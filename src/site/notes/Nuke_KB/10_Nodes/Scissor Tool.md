---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/scissor-tool/","tags":["nuke","node","organization","efficiency"]}
---

Compatible Nuke versions: 13.0 or later
https://www.nukepedia.com/tools/python/nodegraph/scissor-tool/

## 🧠 Description

Scissor tool is a simple UI Widget activated by a shortcut. Includes Preferences to change the shortcut (Change requires nuke restart), line color and thickness.
## 🎯 When to use


## ⚠️ Known pitfalls

Tested on Windows and Linux. This does not currently work inside opened expanded group views.

## 🧩 Dependencies

simply place the ScissorTool.py file in your .nuke folder and then add the following line into your menu.py file:

```
import  ScissorTool
```

## 🧪 Alternatives

## 🧾 Code

```
import nuke, threading, platform, os

if nuke.NUKE_VERSION_MAJOR < 16:
    from PySide2.QtWidgets import *
    from PySide2.QtCore import *
    from PySide2.QtGui import *
else:
    from PySide6.QtWidgets import *
    from PySide6.QtCore import *
    from PySide6.QtGui import *


def savePreferencesToFile():
    nukeFolder = os.path.expanduser('~') + '/.nuke/'
    preferencesFile = nukeFolder + 'preferences%i.%i.nk' %(nuke.NUKE_VERSION_MAJOR,nuke.NUKE_VERSION_MINOR)

    preferencesNode = nuke.toNode('preferences')

    customPrefences = preferencesNode.writeKnobs( nuke.WRITE_USER_KNOB_DEFS | nuke.WRITE_NON_DEFAULT_ONLY | nuke.TO_SCRIPT | nuke.TO_VALUE )
    customPrefences = customPrefences.replace('\n','\n  ')

    preferencesCode = 'Preferences {\n inputs 0\n name Preferences%s\n}' %customPrefences

    # write to file
    openPreferencesFile = open( preferencesFile , 'w' )
    openPreferencesFile.write( preferencesCode )
    openPreferencesFile.close()

#create preferences knobs
prefNode = nuke.toNode('preferences')

if prefNode.knob('Scissor Tool') is None:
    prefNode.addKnob(nuke.Tab_Knob('Scissor Tool'))

    textKnob = nuke.EvalString_Knob('scissor tool shortcut')
    textKnob.setValue('\\')
    prefNode.addKnob(textKnob)

    colorKnob = nuke.Color_Knob('scissor color')
    colorKnob.setValue([1,1,1])
    prefNode.addKnob(colorKnob)

    intKnob = nuke.Int_Knob('scissor line thickness')
    intKnob.setValue(1)
    prefNode.addKnob(intKnob)

    savePreferencesToFile()

if prefNode.knob('scissor tool shortcut') is not None:
    prefNode['scissor tool shortcut'].setFlag(0x0000000002000000)

#get shortcut
shortcut = prefNode['scissor tool shortcut'].value()

#switch that keeps the ui from reopening
canOpen = True


def ccw(A,B,C):
    return (C[1]-A[1])*(B[0]-A[0]) > (B[1]-A[1])*(C[0]-A[0])

def intersect(A,B,C,D):
    return ccw(A,C,D) != ccw(B,C,D) and ccw(A,B,C) != ccw(A,B,D)

def get_DAG_widget():
    stack = QApplication.topLevelWidgets()

    #return which DAG window the mouse is over, if any
    for widget in stack:
        try:
            stack.extend([i for i in widget.children() if i is not None])
            if widget.objectName()[:3] == 'DAG' and widget.underMouse():
                if nuke.NUKE_VERSION_MAJOR < 16:
                    return widget
                else:
                    return widget.parent()
        except:
            pass

def DAGMousePos(dag_widget):
    if dag_widget is None:
        return None
    dag_center = QPoint(*nuke.center())
    cursor_pos = QCursor().pos()
    dag_widget_center = QPoint((dag_widget.size().width()/ 2), (dag_widget.size().height()/2) )
    cursor_pos_in_dag = dag_widget.mapFromGlobal(cursor_pos)

    return ((cursor_pos_in_dag - dag_widget_center) / nuke.zoom() + dag_center).toTuple()

class Panel(QWidget):
    finished = Signal()
    def __init__(self):
        super(Panel, self).__init__()

        if platform.system() == 'Windows':
            self.installEventFilter(self)

        #define DAG widget
        global DAG
        self.DAG = DAG

        #find DAG position and size
        screenSize = self.DAG.size()
        self.screenPos = self.DAG.mapToGlobal( QPoint(0,0) )

        #set window  position and size
        self.resize( screenSize )
        self.move( self.screenPos )

        #make window transparent
        self.setWindowFlags(Qt.FramelessWindowHint | Qt.WindowStaysOnTopHint | Qt.SubWindow)
        self.setAttribute(Qt.WA_NoSystemBackground)
        self.setAttribute(Qt.WA_TranslucentBackground)

        #prevent window beeing bound to screen edge in linux
        if platform.system() == 'Linux':
            self.setAttribute(Qt.WA_X11NetWmWindowTypeDock)

        #force keyboard events to widget
        self.grabKeyboard()
        self.setWindowModality(Qt.ApplicationModal)

        #initialise cursor position
        self.start = QCursor().pos() - self.screenPos
        self.mouse_position = self.start

        #set timer for cursor polling
        self.timer = QTimer(self)
        self.timer.timeout.connect(self.pollCursor)
        self.timer.start()

        #open UI
        self.finished.connect(self.show)
        thread = threading.Thread(target=self.run)
        thread.daemon = True
        thread.start()

    def run(self):
        #get context
        parentName = self.DAG.windowTitle()[:-11]

        if parentName == '':
            self.parent = nuke.root()
        else:
            self.parent = nuke.toNode('root.' + parentName)

        #save starting mouse position
        with self.parent:
            self.a = DAGMousePos(self.DAG)

        self.finished.emit()

    #keeps the ui from reopening
    def keyPressEvent(self,event):
        if event.isAutoRepeat():
            return
    def keyReleaseEvent(self,event):
        if event.isAutoRepeat():
            return
        if event.text() == shortcut:
            self.closeWidget()
            return
    #focus deactivation
    def eventFilter(self, object, event):
        if event.type() in [QEvent.WindowDeactivate, QEvent.FocusOut] and not canOpen:
            self.closeWidget()
            return True
        return False

    #polling of the cursor
    def pollCursor(self):
        self.mouse_position = QCursor().pos() - self.screenPos
        self.update()

    #create the line
    def paintEvent(self, QPaintEvent):
        super(Panel, self).paintEvent(QPaintEvent)

        painter = QPainter(self)
        cv = [int(i*255) for i in prefNode['scissor color'].array()]
        pen = QPen(QColor(cv[0],cv[1],cv[2]))
        pen.setWidth(int(prefNode['scissor line thickness'].value()))
        pen.setStyle(Qt.DotLine)
        painter.setPen(pen)
        painter.setRenderHint(QPainter.Antialiasing)

        line = QLine(self.start, self.mouse_position)
        painter.drawLine(line)


    def closeWidget(self):
        global canOpen
        canOpen = True
        #close widget
        self.close()
        #stop the polling timer
        self.timer.stop()
        #release keyboard
        self.releaseKeyboard()

        ###### ----- run intersect code ----- #####

        undo = nuke.Undo()
        undo.begin("Scissor Tool")

        with self.parent:
            b = DAGMousePos(self.DAG)
            a = self.a

            #find all connections
            for nodeA in nuke.allNodes():
                connections = nodeA.dependencies(nuke.INPUTS)

                if len(connections) > 0:
                    c = (nodeA.xpos()+(nodeA.screenWidth()/2),nodeA.ypos()+(nodeA.screenHeight()/2))

                    for nodeB in connections:
                        d = (nodeB.xpos()+(nodeB.screenWidth()/2),nodeB.ypos()+(nodeB.screenHeight()/2))
                        #check for intersection with scissor line
                        if intersect(a,b,c,d):
                            for i in range(nodeA.inputs()):
                                #disconnect if current input is connected to other node
                                if nodeA.input(i) is nodeB:
                                    nodeA.setInput(i, None)
        undo.end()

def run():
    global canOpen
    if canOpen:
        global DAG
        DAG = get_DAG_widget()
        if DAG is not None:
            #initialize panel
            run.panel = Panel()
            #turns off canOpen so the ui doesnt reopen
            canOpen = False


menubar = nuke.menu('Nuke')
menubar.addCommand('@;Scissor tool', run , shortcut)

```