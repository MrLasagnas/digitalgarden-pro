---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/match-intensity/","tags":["nuke","node"]}
---



## 🧠 Description


## 🎯 When to use


## ⚠️ Known pitfalls

## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```
set cut_paste_input [stack 0]  
version 13.0 v10  
push $cut_paste_input  
Group {  
name MatchIntensity  
tile_color 0x85ade5ff  
selected true  
xpos 3283  
ypos -4891  
icon //stora/diska/global/templatesProd/Other_images/F_iconrgb.png  
addUserKnob {20 MatchIntensiy}  
addUserKnob {41 channels T CurveTool_Intensity.channels}  
addUserKnob {26 ""}  
addUserKnob {41 go_1 l Analyse T CurveTool_Intensity.go}  
addUserKnob {4 Mode l " Mode " -STARTLINE M {"Add intensity Value ( match )" "Remove intensity Value ( compensate )" "Analyse View" "" ""}}  
Mode "Analyse View"  
addUserKnob {3 RefFrame l "Reference Frame"}  
addUserKnob {22 SetFrame l "Set to this frame" -STARTLINE T nuke.thisNode().knob('RefFrame').setValue(nuke.frame())}  
addUserKnob {26 ""}  
addUserKnob {20 AnalyseSettings l "Analyse Settings" n 1}  
AnalyseSettings 0  
addUserKnob {41 avgframes l "# frames for base average" T CurveTool_Intensity.avgframes}  
addUserKnob {41 ROI l "Zone for analyse" T CurveTool_Intensity.ROI}  
addUserKnob {41 resetROI l Reset -STARTLINE T CurveTool_Intensity.resetROI}  
addUserKnob {20 endGroup_2 l endGroup n -1}  
addUserKnob {20 Curve n 1}  
Curve 0  
addUserKnob {19 CT l INVISIBLE -STARTLINE +INVISIBLE}  
CT {{CurveTool_Intensity.intensitydata} {CurveTool_Intensity.intensitydata} {CurveTool_Intensity.intensitydata} {CurveTool_Intensity.intensitydata}}  
addUserKnob {6 CT_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}  
addUserKnob {41 intensitydata T CurveTool_Intensity.intensitydata}  
addUserKnob {20 endGroup n -1}  
addUserKnob {26 ""}  
addUserKnob {22 CrtLnkGrd l "<font color = \"springGreen\"> Create Grade (Linked)" T "groupNode = nuke.thisNode()\nname = groupNode.name()\n\nC = nuke.createNode(\"Grade\", inpanel=False )\n\nC\['tile_color'].setValue(0x82e3ffff)\nC\['label'].setValue(\"(\" +name + \")\")\n\n\n#Create new knob#\nt= nuke.Tab_Knob ('CtrlTab', 'Ctrl')\nj= nuke.Int_Knob ('Ref' , 'Reference Frame')\nk= nuke.Enumeration_Knob ( 'mode' , 'Mode' , \['Add intensity ( match )','Remove intensity ( compensate )' ] )\nl = nuke.PyScript_Knob(\"SetRef\", \"Set to this frame\" , \"nuke.thisNode().knob('Ref').setValue(nuke.frame())\")\nC.addKnob(t)\nC.addKnob(k)\nC.addKnob(j)\nC.addKnob(l)\n\n#Set expression#\nC\['white'].setSingleValue(False)\nC\['white'].setExpression(\"((\"+name+\".CT)==(-0) || (\"+name+\".CT)==0?0.00001:(\"+name+\".CT))/((\"+name+\".CT(Ref))==0?1:(\"+name+\".CT(Ref)))\")\nC\['reverse'].setExpression(\"mode\")\nC\['Ref'].setExpression(name + \".RefFrame\")\n\nC.knob('Ref').clearAnimated()\n\nC.setSelected(True)\n\nnuke.nodeCopy('%clipboard%')\n\nn = nuke.selectedNode()\nnuke.delete(n)\n\nnuke.selectAll() \nnuke.invertSelection()\n\nnodePos = ( nuke.thisNode().xpos(), nuke.thisNode().ypos()) \n\ngroupNode.end()\n\nfor node in nuke.allNodes():\n        node.setSelected(False)\n\nnuke.nodePaste('%clipboard%')\n\ngroupNode.setSelected(False)\n\nnodesToPlace = sorted( nuke.selectedNodes(), key=lambda node: node.ypos())\n\nfor index,node in enumerate(nodesToPlace):\n        node.setXYpos(nodePos\[0]+100, nodePos\[1])\n" +STARTLINE}  
addUserKnob {22 CrtBkdGrd l INVISIBLE -STARTLINE +INVISIBLE T "groupNode = nuke.thisNode()\nname = groupNode.name()\n\nC = nuke.createNode(\"Grade\", inpanel=False )\n\nC\['tile_color'].setValue(0x82e3ffff)\nC\['label'].setValue(\"(\" +name + \")\")\n\n\n#Create new knob#\nt= nuke.Tab_Knob ('CtrlTab', 'Ctrl')\nj= nuke.Int_Knob ('Ref' , 'Reference Frame')\nk= nuke.Enumeration_Knob ( 'mode' , 'Mode' , \['Add intensity ( match )','Remove intensity ( compensate )' ] )\nl = nuke.PyScript_Knob(\"SetRef\", \"Set to this frame\" , \"nuke.thisNode().knob('Ref').setValue(nuke.frame())\")\nC.addKnob(t)\nC.addKnob(k)\nC.addKnob(j)\nC.addKnob(l)\n\n#Set expression#\nC\['whitepoint'].setExpression(\"intensitydata(Ref)\")\nC\['white'].setExpression(name + \".intensitydata\")\nC\['reverse'].setExpression(\"mode\")\nC\['Ref'].setExpression(name + \".RefFrame\")\n\n\nC.knob('Ref').clearAnimated()\nnuke.animation( \"Grade12.white\" , \"generate\", (\"0\", \"100\", \"1\", \"y\", \"white\"))\nC.setSelected(True)\n\nnuke.nodeCopy('%clipboard%')\n\nn = nuke.selectedNode()\nnuke.delete(n)\n\nnuke.selectAll() \nnuke.invertSelection()\n\nnodePos = ( nuke.thisNode().xpos(), nuke.thisNode().ypos()) \n\ngroupNode.end()\n\nfor node in nuke.allNodes():\n        node.setSelected(False)\n\nnuke.nodePaste('%clipboard%')\n\ngroupNode.setSelected(False)\n\nnodesToPlace = sorted( nuke.selectedNodes(), key=lambda node: node.ypos())\n\nimport nuke\n\ndef bakeCurve( curve, first, last, inc ):\n    '''bake an expresison curve into a keyframes curve'''\n    for f in xrange( first, last, inc ):\n        curve.setKey( f, curve.evaluate( f ) )\n    curve.setExpression( 'curve' )\n\n####################################################\ndef getCurves( knob, views ):\n    '''return a list of all animation curves found in the given knob'''\n    curves = \[]\n    for v in views:\n        curves.extend( knob.animations( v ) )\n    return curves\n\n####################################################\ndef bakeExpressionKnobs( node, first, last, inc, views ):  \n    '''bake all knobs in node that carry expressions'''\n    # GET ALL KNOBS WITH EXPRESSIONS IN THEM\n    expKnobs = \[ k for k in node.knobs().values() if k.hasExpression() ]\n\n    # GET ALL CURVES INSIDE THAT KNOB INCLUDING SPLIT FIELDS AND VIEWS\n    allCurves = \[]\n    for k in expKnobs:\n        allCurves += getCurves( k, views )\n\n    # BAKE ALL CURVES\n    for c in allCurves:\n        bakeCurve( c, first, last, inc )\n        \n####################################################\ndef bakeSelectedNodes():\n    '''bake selected nodes' knobs that carry expressions'''\n    ret = nuke.getFramesAndViews( 'bake curves in selected nodes?', '%s-%s' % (nuke.root().firstFrame(), nuke.root().lastFrame()) )\n    if not ret:\n        return\n    fRange = nuke.FrameRange( ret\[0] )\n    views = ret\[1]\n\n    for n in nuke.selectedNodes():\n        bakeExpressionKnobs( n, fRange.first(), fRange.last(), fRange.increment(), views )\n\nbakeSelectedNodes()\nfor index,node in enumerate(nodesToPlace):\n        node.setXYpos(nodePos\[0]+100, nodePos\[1])\n\nC\['whitepoint'].setExpression(\"white(Ref)\")"}  
addUserKnob {26 "" l mask}  
addUserKnob {41 maskChannelMask l "" -STARTLINE T GradeMatchIntensity.maskChannelMask}  
addUserKnob {41 inject -STARTLINE T GradeMatchIntensity.inject}  
addUserKnob {41 invert_mask l invert -STARTLINE T GradeMatchIntensity.invert_mask}  
addUserKnob {41 fringe -STARTLINE T GradeMatchIntensity.fringe}  
addUserKnob {41 mix T GradeMatchIntensity.mix}  
addUserKnob {1 output l INVISIBLE +INVISIBLE}  
output "RefFrame \[value RefFrame]"  
}  
Input {  
inputs 0  
name Input  
xpos -648  
ypos 411  
}  
Dot {  
name Dot1  
xpos -612  
ypos 473  
}  
set N978f6a20 [stack 0]  
Dot {  
name Dot3  
xpos -780  
ypos 473  
}  
CurveTool {  
ROI {512 389 1536 1167}  
autocropdata {512 389 1536 1167}  
intensitydata {0 0 0 0}  
name CurveTool_Intensity  
xpos -815  
ypos 522  
}  
Dot {  
name Dot2  
xpos -780  
ypos 605  
}  
Input {  
inputs 0  
name InputMask  
xpos -508  
ypos 538  
number 1  
}  
push $N978f6a20  
Grade {  
inputs 1+1  
white {{"((CurveTool_Intensity.intensitydata)==(-0) || (CurveTool_Intensity.intensitydata)==0?0.00001:(CurveTool_Intensity.intensitydata))/((CurveTool_Intensity.intensitydata(RefFrame))==0?1:(CurveTool_Intensity.intensitydata(RefFrame)))"} {"((CurveTool_Intensity.intensitydata)==(-0) || (CurveTool_Intensity.intensitydata)==0?0.00001:(CurveTool_Intensity.intensitydata))/((CurveTool_Intensity.intensitydata(RefFrame))==0?1:(CurveTool_Intensity.intensitydata(RefFrame)))"} {"((CurveTool_Intensity.intensitydata)==(-0) || (CurveTool_Intensity.intensitydata)==0?0.00001:(CurveTool_Intensity.intensitydata))/((CurveTool_Intensity.intensitydata(RefFrame))==0?1:(CurveTool_Intensity.intensitydata(RefFrame)))"} {"((CurveTool_Intensity.intensitydata)==(-0) || (CurveTool_Intensity.intensitydata)==0?0.00001:(CurveTool_Intensity.intensitydata))/((CurveTool_Intensity.intensitydata(RefFrame))==0?1:(CurveTool_Intensity.intensitydata(RefFrame)))"}}  
reverse {{Mode}}  
black_clamp false  
name GradeMatchIntensity  
xpos -648  
ypos 538  
}  
set Neeb2e0f0 [stack 0]  
push $Neeb2e0f0  
Switch {  
inputs 3  
which {{Mode}}  
name Switch_mode  
label "\[value which]"  
xpos -648  
ypos 595  
}  
Output {  
name Output1  
xpos -648  
ypos 658  
}  
end_group
```