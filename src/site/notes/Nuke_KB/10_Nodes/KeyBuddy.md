---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/key-buddy/","tags":["nuke","node","keying"]}
---


# KeyBuddy

## 🧠 Description


## 🎯 When to use


## ⚠️ Known pitfalls


## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```nuke
    set cut_paste_input [stack 0]
    version 13.0 v10
    push $cut_paste_input
    Group {
     name PA_KeyBuddy1
     knobChanged "\n\n\n\n\n\n\n\n#KEY BUDDY\n#CREATED BY PEDRO ANDRADE\n#2017\n\n\n#AUTO VS CUSTOM\n\nif nuke.thisNode().knob('manual').value() == True:\n    nuke.thisNode().knob('calculate').setEnabled(False)\n    nuke.thisNode().knob('show_clean_plate').setVisible(True)\n    nuke.thisNode().knob('color_sample').setVisible(True)\n\n\n\nelse:\n    nuke.thisNode().knob('calculate').setEnabled(True)   \n    nuke.thisNode().knob('show_clean_plate').setVisible(False)\n    nuke.thisNode().knob('color_sample').setVisible(False)\n\n    nuke.thisNode().knob('darks').setVisible(True)\n    nuke.thisNode().knob('lights').setVisible(True)\n\n\n\n\n#OVERALL\n\n#CLEAN PLATE MODE\n\nif nuke.thisNode().knob('viewer').value() == 'Clean Plate':\n    nuke.thisNode().knob('darks').setVisible(True)\n    nuke.thisNode().knob('lights').setVisible(True)\n    nuke.thisNode().knob('erode').setVisible(True)\n    nuke.thisNode().knob('cleaning_iterations').setVisible(True)\n\n    nuke.thisNode().knob('calculate').setVisible(False)\n    nuke.thisNode().knob('manual').setVisible(False)   \n    nuke.thisNode().knob('show_clean_plate').setVisible(False)\n    nuke.thisNode().knob('color_sample').setVisible(False)\n\n    nuke.thisNode().knob('matte_blacks').setVisible(False)\n    nuke.thisNode().knob('matte_whites').setVisible(False)\n\n\n\n#EVEN SCREEN MODE\n\nif nuke.thisNode().knob('viewer').value() == 'Even Screen':\n    nuke.thisNode().knob('darks').setVisible(False)\n    nuke.thisNode().knob('lights').setVisible(False)\n    nuke.thisNode().knob('erode').setVisible(False)\n    nuke.thisNode().knob('cleaning_iterations').setVisible(False)\n\n    nuke.thisNode().knob('calculate').setVisible(True)\n    nuke.thisNode().knob('manual').setVisible(True)   \n\n    nuke.thisNode().knob('matte_blacks').setVisible(True)\n    nuke.thisNode().knob('matte_whites').setVisible(True)\n\n\n\n#MATTE MODE\n\nif nuke.thisNode().knob('viewer').value() == 'Matte':\n    nuke.thisNode().knob('darks').setVisible(False)\n    nuke.thisNode().knob('lights').setVisible(False)\n    nuke.thisNode().knob('erode').setVisible(False)\n    nuke.thisNode().knob('cleaning_iterations').setVisible(False)\n    \n    nuke.thisNode().knob('calculate').setVisible(False)\n    nuke.thisNode().knob('manual').setVisible(False)   \n    nuke.thisNode().knob('show_clean_plate').setVisible(False)\n    nuke.thisNode().knob('color_sample').setVisible(False)\n\n\n    nuke.thisNode().knob('matte_blacks').setVisible(True)\n    nuke.thisNode().knob('matte_whites').setVisible(True)\n\n\n\n#MATTE INPUT / CLEAN PLATE\n\nif nuke.thisNode().knob('matte_input').value() == True:\n    nuke.thisNode().knob('darks').setEnabled(False)\n    nuke.thisNode().knob('lights').setEnabled(False)\n    nuke.thisNode().knob('erode').setEnabled(False)\n    nuke.thisNode().knob('screen_type').setEnabled(False)\n\nelse:\n    nuke.thisNode().knob('darks').setEnabled(True)\n    nuke.thisNode().knob('lights').setEnabled(True)\n    nuke.thisNode().knob('erode').setEnabled(True)\n    nuke.thisNode().knob('screen_type').setEnabled(True)\n\n\n#DYNAMIC INPUTS\n#MATTE INPUT\n\nif nuke.thisNode().knob('matte_input').value() == True:\n\n    for n in nuke.allNodes('Input'):\n        if n.knob('name').value() == 'matte_input':\n            matteInputXPos = n.knob('xpos').value()\n            matteInputYPos = n.knob('ypos').value()\n            nuke.nodes.StickyNote(name = '_StickyNote_1', label = str(matteInputXPos) + ',' + str(matteInputYPos))\n            nuke.delete(n)\n                          \n    dotMatteInput = nuke.nodes.Input(name='matte_input', xpos = nuke.toNode('_StickyNote_1').knob('label').value().split(',')\[0], ypos = nuke.toNode('_StickyNote_1').knob('label').value().split(',')\[1])\n    nuke.delete(nuke.toNode('_StickyNote_1'))\n    for n in nuke.allNodes('Dot'):\n        if n.knob('label').value() == 'dot_matte_input':\n            n.setInput(0, dotMatteInput)\n\n\n    for n in nuke.allNodes('Input'):\n        if n.knob('name').value() == 'additional_matte':\n            additionalMatteInputXPos = n.knob('xpos').value()\n            additionalMatteInputYPos = n.knob('ypos').value()\n            nuke.nodes.StickyNote(name = '_StickyNote_3', label = str(additionalMatteInputXPos) + ',' + str(additionalMatteInputYPos))\n            nuke.delete(n)\n            \n\n\nelse:\n    for n in nuke.allNodes('Input'):\n        if n.knob('name').value() == 'matte_input':\n            matteInputXPos = n.knob('xpos').value()\n            matteInputYPos = n.knob('ypos').value()\n            nuke.nodes.StickyNote(name = '_StickyNote_1', label = str(matteInputXPos) + ',' + str(matteInputYPos))\n            nuke.delete(n)\n\n\n\n    for n in nuke.allNodes('Input'):\n        if n.knob('name').value() == 'additional_matte':\n            additionalMatteInputXPos = n.knob('xpos').value()\n            additionalMatteInputYPos = n.knob('ypos').value()\n            nuke.nodes.StickyNote(name = '_StickyNote_3', label = str(additionalMatteInputXPos) + ',' + str(additionalMatteInputYPos))\n            nuke.delete(n)\n                          \n    additionalMatteInput = nuke.nodes.Input(name='additional_matte', xpos = nuke.toNode('_StickyNote_3').knob('label').value().split(',')\[0], ypos = nuke.toNode('_StickyNote_3').knob('label').value().split(',')\[1])\n    nuke.delete(nuke.toNode('_StickyNote_3'))\n    for n in nuke.allNodes('Dot'):\n        if n.knob('label').value() == 'dot_additional_matte':\n            n.setInput(0, additionalMatteInput)\n\n\n\n#CLEAN PLATE INPUT\nif nuke.thisNode().knob('clean_plate_input').value() == True:\n    for n in nuke.allNodes('Input'):\n        if n.knob('name').value() == 'clean_plate':\n            cleanPlateInputXPos = n.knob('xpos').value()\n            cleanPlateInputYPos = n.knob('ypos').value()\n            nuke.nodes.StickyNote(name = '_StickyNote_2', label = str(cleanPlateInputXPos) + ',' + str(cleanPlateInputYPos))\n            nuke.delete(n)\n\n                          \n    dotCleanPlateInput = nuke.nodes.Input(name='clean_plate', xpos = nuke.toNode('_StickyNote_2').knob('label').value().split(',')\[0], ypos = nuke.toNode('_StickyNote_2').knob('label').value().split(',')\[1])\n    nuke.delete(nuke.toNode('_StickyNote_2'))\n    for n in nuke.allNodes('Dot'):\n        if n.knob('label').value() == 'dot_clean_plate_input':\n            n.setInput(0, dotCleanPlateInput)\n\nelse:\n    for n in nuke.allNodes('Input'):\n        if n.knob('name').value() == 'clean_plate':\n            cleanPlateInputXPos = n.knob('xpos').value()\n            cleanPlateInputYPos = n.knob('ypos').value()\n            nuke.nodes.StickyNote(name = '_StickyNote_2', label = str(cleanPlateInputXPos) + ',' + str(cleanPlateInputYPos))\n            nuke.delete(n)\n\n"
     tile_color 0xff5f00ff
     label "\[value viewer]\n"
     note_font "Bitstream Vera Sans Bold"
     selected true
     xpos -26770
     ypos 8101
     addUserKnob {20 User}
     addUserKnob {4 screen_type l "Screen Preset" M {Green Blue "" ""}}
     screen_type Blue
     addUserKnob {6 matte_input l "Matte Input" -STARTLINE}
     addUserKnob {6 clean_plate_input l "Clean Plate Input" -STARTLINE}
     addUserKnob {26 ""}
     addUserKnob {4 viewer l Viewer M {"Clean Plate" "Even Screen" Matte "" ""}}
     addUserKnob {22 calculate l Calculate -STARTLINE +HIDDEN T "with nuke.thisNode():\n    for n in nuke.allNodes('CurveTool'):\n        n.knob('go').execute()"}
     addUserKnob {7 matte_blacks l "Matte Blacks" +HIDDEN R -1 1}
     addUserKnob {7 matte_whites l "Matte Whites" +HIDDEN R 0 4}
     matte_whites 1
     addUserKnob {6 manual l Manual +HIDDEN +STARTLINE}
     addUserKnob {6 show_clean_plate l "Show Clean Plate" -STARTLINE +HIDDEN}
     addUserKnob {18 color_sample l "Color Sample" +HIDDEN}
     color_sample {1 1 1}
     addUserKnob {6 color_sample_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
     addUserKnob {6 color_sample_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
     addUserKnob {6 color_sample_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
     addUserKnob {18 darks l Darks}
     darks {0 0 0}
     addUserKnob {6 darks_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
     addUserKnob {6 darks_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
     addUserKnob {6 darks_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
     addUserKnob {18 lights l Lights}
     lights {1.6 1.1 0.18}
     addUserKnob {6 lights_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
     addUserKnob {6 lights_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
     addUserKnob {6 lights_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
     addUserKnob {7 erode l Erode R 0 10}
     erode 5
     addUserKnob {7 cleaning_iterations l "Patch Black" R 0 12}
     cleaning_iterations 8
     addUserKnob {26 ""}
     addUserKnob {26 author l "" +STARTLINE T "Key Buddy v2.2\nCreated by Pedro Andrade\n-2017-"}
    }
     BackdropNode {
      inputs 0
      name BackdropNode19
      tile_color 0x378e3eff
      label GREEN
      note_font_size 42
      xpos 901
      ypos -447
      bdwidth 453
      bdheight 2308
      z_order -1
     }
     BackdropNode {
      inputs 0
      name BackdropNode20
      tile_color 0x37558eff
      label BLUE
      note_font_size 42
      xpos 1830
      ypos -447
      bdwidth 540
      bdheight 2265
      z_order -1
     }
     BackdropNode {
      inputs 0
      name BackdropNode3
      tile_color 0x8e8e8eff
      label "MATTE INPUT"
      note_font_size 42
      xpos 2929
      ypos -460
      bdwidth 448
      bdheight 2309
      z_order -1
     }
     BackdropNode {
      inputs 0
      name BackdropNode1
      tile_color 0x71c67100
      label "SCREEN TYPE"
      note_font_size 42
      xpos 2050
      ypos 1983
      bdwidth 327
      bdheight 136
     }
     BackdropNode {
      inputs 0
      name BackdropNode10
      tile_color 0x8e388e00
      label "FROM INPUT"
      note_font_size 42
      xpos 1550
      ypos 3787
      bdwidth 275
      bdheight 124
     }
     BackdropNode {
      inputs 0
      name BackdropNode15
      tile_color 0x388e8e00
      label ITERATIONS
      note_font_size 42
      xpos 1023
      ypos 414
      bdwidth 216
      bdheight 1069
     }
     BackdropNode {
      inputs 0
      name BackdropNode2
      tile_color 0x8e8e3800
      label "VIEW MODE"
      note_font_size 42
      xpos 2050
      ypos 4873
      bdwidth 340
      bdheight 163
     }
     BackdropNode {
      inputs 0
      name BackdropNode22
      tile_color 0x8e388e00
      label CORE
      note_font_size 42
      xpos 1023
      ypos -356
      bdwidth 154
      bdheight 130
     }
     BackdropNode {
      inputs 0
      name BackdropNode23
      tile_color 0x388e8e00
      label ITERATIONS
      note_font_size 42
      xpos 1951
      ypos 401
      bdwidth 216
      bdheight 1069
     }
     BackdropNode {
      inputs 0
      name BackdropNode24
      tile_color 0x8e388e00
      label CORE
      note_font_size 42
      xpos 1951
      ypos -369
      bdwidth 154
      bdheight 130
     }
     BackdropNode {
      inputs 0
      name BackdropNode295
      tile_color 0x388e8e00
      label "FORCED STENCIL"
      note_font "Bitstream Vera Sans"
      note_font_size 42
      xpos 181
      ypos -229
      bdwidth 365
      bdheight 178
     }
     BackdropNode {
      inputs 0
      name BackdropNode4
      tile_color 0x388e8e00
      label ITERATIONS
      note_font_size 42
      xpos 3050
      ypos 388
      bdwidth 216
      bdheight 1069
     }
     BackdropNode {
      inputs 0
      name BackdropNode5
      tile_color 0x8e388e00
      label CORE
      note_font_size 42
      xpos 3050
      ypos -381
      bdwidth 154
      bdheight 130
     }
     BackdropNode {
      inputs 0
      name BackdropNode6
      tile_color 0xaaaaaa00
      label "EVEN SCREEN"
      note_font_size 42
      xpos 1905
      ypos 3357
      bdwidth 545
      bdheight 854
     }
     BackdropNode {
      inputs 0
      name BackdropNode7
      tile_color 0xaaaaaa00
      label "MATTE INPUT"
      note_font_size 42
      xpos 2050
      ypos 2227
      bdwidth 353
      bdheight 165
     }
     BackdropNode {
      inputs 0
      name BackdropNode8
      tile_color 0x8e8e3800
      label "CRANK ALPHA"
      note_font_size 42
      xpos 1250
      ypos 3352
      bdwidth 313
      bdheight 138
     }
     BackdropNode {
      inputs 0
      name BackdropNode9
      tile_color 0x388e8e00
      label "CLEAN PLATE INPUT"
      note_font_size 42
      xpos 2050
      ypos 2500
      bdwidth 419
      bdheight 164
     }
     Dot {
      inputs 0
      name dot_clean_plate_input
      label dot_clean_plate_input
      xpos 2194
      ypos 2580
     }
     Input {
      inputs 0
      name additional_matte
      xpos 460
      ypos -476
      number 1
     }
     Dot {
      name dot_additional_matte
      label dot_additional_matte
      xpos 494
      ypos -406
     }
     Shuffle {
      in alpha
      name Shuffle2
      label "\[value in]"
      xpos 460
      ypos -316
     }
     Expression {
      channel3 rgba
      expr3 a>0?1:0
      name Expression25
      xpos 460
      ypos -108
     }
     Dot {
      name Dot33
      xpos 933
      ypos -104
     }
    set N1b0b5880 [stack 0]
     Dot {
      name Dot38
      xpos 1860
      ypos -104
     }
    set N1b0ba5d0 [stack 0]
     Dot {
      name Dot40
      xpos 2994
      ypos -104
     }
     Dot {
      name Dot41
      xpos 2994
      ypos -6
     }
     Dot {
      inputs 0
      name dot_matte_input
      label dot_matte_input
      xpos 3094
      ypos -1356
     }
     Shuffle {
      in alpha
      name Shuffle3
      label "\[value in]"
      xpos 3060
      ypos -1263
     }
     Expression {
      expr3 a>0?1:0
      name Expression2
      xpos 3060
      ypos -1179
     }
     Dot {
      name Dot37
      xpos 3094
      ypos -916
     }
     Shuffle {
      alpha white
      name Shuffle4
      label "\[value in]"
      xpos 3060
      ypos -771
      disable {{"\[exists parent.input0]"}}
     }
    set N1b0f1b20 [stack 0]
     Input {
      inputs 0
      name plate
      xpos 1961
      ypos -859
     }
     Shuffle {
      alpha white
      name Shuffle7
      label "\[value in]"
      xpos 1961
      ypos -809
      disable {{"\[exists parent.input0]"}}
     }
     Dot {
      name Dot1
      xpos 1995
      ypos -665
     }
    set N1b118140 [stack 0]
     Dot {
      name Dot25
      xpos 2394
      ypos -665
     }
    set N1b11cfe0 [stack 0]
     Dot {
      name Dot23
      xpos 2734
      ypos -665
     }
    set N1b121f10 [stack 0]
     Dot {
      name Dot24
      xpos 2734
      ypos -286
     }
     Group {
      inputs 2
      name IBKColourV3_1
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 3060
      ypos -295
      addUserKnob {20 "" l Parameters}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_6.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      erode {{parent.IBKColourV3_6.erode}}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{parent.IBKColourV3_6.multi}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name matte_input
       xpos -740
       ypos -430
       number 1
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -323
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos -226
      }
      Expression {
       expr3 r+g+b
       name Expression1
       xpos -740
       ypos -54
      }
      Expression {
       expr3 a>0?1:0
       name Expression2
       xpos -740
       ypos 18
      }
      Invert {
       channels alpha
       name Invert1
       xpos -740
       ypos 124
      }
      Dot {
       name Dot1
       xpos -706
       ypos 274
      }
      Dot {
       name Dot2
       xpos 16
       ypos 274
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1b1a2310 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1b1a2310
      Input {
       inputs 0
       name Input1
       xpos -340
       ypos -850
      }
      Dot {
       name Dot16
       xpos -306
       ypos -192
      }
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop1
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1b1f24f0 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1b1fa9a0 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1b1fa9a0
    push $N1b1f24f0
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1b20e3b0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1b245ae0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1b245ae0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1b20e3b0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Output {
       name Output1
       xpos 233
       ypos 1950
      }
     end_group
     Merge2 {
      inputs 2
      operation stencil
      name Merge3
      xpos 3060
      ypos -10
     }
     Dot {
      name Dot44
      xpos 3094
      ypos 134
     }
    set N1b2be4f0 [stack 0]
     Dot {
      name Dot34
      xpos 3194
      ypos 134
     }
     Crop {
      box {0 0 {input.width} {input.height}}
      name Crop4
      xpos 3187
      ypos 1590
     }
     FilterErode {
      size 20
      filter gaussian
      name FilterErode3
      xpos 3187
      ypos 1662
     }
    push $N1b121f10
     Dot {
      name Dot5
      xpos 3421
      ypos -665
     }
     Dot {
      name Dot36
      xpos 3421
      ypos 1717
     }
    push $N1b2be4f0
    push $N1b2be4f0
     Group {
      inputs 2
      name IBKColourV3_2
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 3060
      ypos 474
      addUserKnob {20 "" l Parameters}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_1.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi 1
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name matte_input
       xpos -740
       ypos -430
       number 1
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -323
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos -226
      }
      Expression {
       expr3 r+g+b
       name Expression1
       xpos -740
       ypos -54
      }
      Expression {
       expr3 a>0?1:0
       name Expression2
       xpos -740
       ypos 18
      }
    set N1b33b430 [stack 0]
      Dot {
       name Dot1
       xpos -706
       ypos 274
      }
      Dot {
       name Dot2
       xpos 16
       ypos 274
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1b3532b0 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1b3532b0
      Input {
       inputs 0
       name Input1
       xpos -340
       ypos -850
      }
      Dot {
       name Dot16
       xpos -306
       ypos -192
      }
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1b3a3410 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1b3ab8c0 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1b3ab8c0
    push $N1b3a3410
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1b3bf340 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1b3f6a80 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1b3f6a80
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1b3bf340
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1950
      }
    push $N1b33b430
    push 0
      Viewer {
       inputs 2
       frame_range 1001-1057
       input_number 1
       masking_ratio 2.35
       name Viewer1
       xpos -640
       ypos 550
      }
     end_group
    set N1b2e8cf0 [stack 0]
    push $N1b2e8cf0
     Group {
      inputs 2
      name IBKColourV3_3
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 3060
      ypos 558
      addUserKnob {20 "" l Parameters}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_1.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name matte_input
       xpos -740
       ypos -430
       number 1
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -323
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos -226
      }
      Expression {
       expr3 r+g+b
       name Expression1
       xpos -740
       ypos -54
      }
      Expression {
       expr3 a>0?1:0
       name Expression2
       xpos -740
       ypos 18
      }
    set N1b4e10c0 [stack 0]
      Dot {
       name Dot1
       xpos -706
       ypos 274
      }
      Dot {
       name Dot2
       xpos 16
       ypos 274
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1b4f8f40 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1b4f8f40
      Input {
       inputs 0
       name Input1
       xpos -340
       ypos -850
      }
      Dot {
       name Dot16
       xpos -306
       ypos -192
      }
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1b5490b0 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1b551550 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1b551550
    push $N1b5490b0
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1b564fd0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1b59c700 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1b59c700
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1b564fd0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1950
      }
    push $N1b4e10c0
    push 0
      Viewer {
       inputs 2
       frame_range 1001-1057
       input_number 1
       masking_ratio 2.35
       name Viewer1
       xpos -640
       ypos 550
      }
     end_group
    set N1b48ab90 [stack 0]
    push $N1b48ab90
     Group {
      inputs 2
      name IBKColourV3_4
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 3060
      ypos 642
      addUserKnob {20 "" l Parameters}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_1.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name matte_input
       xpos -740
       ypos -430
       number 1
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -323
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos -226
      }
      Expression {
       expr3 r+g+b
       name Expression1
       xpos -740
       ypos -54
      }
      Expression {
       expr3 a>0?1:0
       name Expression2
       xpos -740
       ypos 18
      }
    set N1b68cd50 [stack 0]
      Dot {
       name Dot1
       xpos -706
       ypos 274
      }
      Dot {
       name Dot2
       xpos 16
       ypos 274
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1b6a4bc0 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1b6a4bc0
      Input {
       inputs 0
       name Input1
       xpos -340
       ypos -850
      }
      Dot {
       name Dot16
       xpos -306
       ypos -192
      }
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1b6f4d20 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1b6fd1c0 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1b6fd1c0
    push $N1b6f4d20
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1b710c40 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1b748370 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1b748370
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1b710c40
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1950
      }
    push $N1b68cd50
    push 0
      Viewer {
       inputs 2
       frame_range 1001-1057
       input_number 1
       masking_ratio 2.35
       name Viewer1
       xpos -640
       ypos 550
      }
     end_group
    set N1b630800 [stack 0]
    push $N1b630800
     Group {
      inputs 2
      name IBKColourV3_5
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 3060
      ypos 726
      addUserKnob {20 "" l Parameters}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_1.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name matte_input
       xpos -740
       ypos -430
       number 1
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -323
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos -226
      }
      Expression {
       expr3 r+g+b
       name Expression1
       xpos -740
       ypos -54
      }
      Expression {
       expr3 a>0?1:0
       name Expression2
       xpos -740
       ypos 18
      }
    set N1b8329d0 [stack 0]
      Dot {
       name Dot1
       xpos -706
       ypos 274
      }
      Dot {
       name Dot2
       xpos 16
       ypos 274
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1b84a7b0 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1b84a7b0
      Input {
       inputs 0
       name Input1
       xpos -340
       ypos -850
      }
      Dot {
       name Dot16
       xpos -306
       ypos -192
      }
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1b89a9c0 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1b8a2e60 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1b8a2e60
    push $N1b89a9c0
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1b8b68e0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1b8ee010 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1b8ee010
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1b8b68e0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1950
      }
    push $N1b8329d0
    push 0
      Viewer {
       inputs 2
       frame_range 1001-1057
       input_number 1
       masking_ratio 2.35
       name Viewer1
       xpos -640
       ypos 550
      }
     end_group
    set N1b7dc490 [stack 0]
    push $N1b7dc490
     Group {
      inputs 2
      name IBKColourV3_8
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 3060
      ypos 810
      addUserKnob {20 "" l Parameters}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_1.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name matte_input
       xpos -740
       ypos -430
       number 1
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -323
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos -226
      }
      Expression {
       expr3 r+g+b
       name Expression1
       xpos -740
       ypos -54
      }
      Expression {
       expr3 a>0?1:0
       name Expression2
       xpos -740
       ypos 18
      }
    set N1b9d8670 [stack 0]
      Dot {
       name Dot1
       xpos -706
       ypos 274
      }
      Dot {
       name Dot2
       xpos 16
       ypos 274
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1b9f04f0 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1b9f04f0
      Input {
       inputs 0
       name Input1
       xpos -340
       ypos -850
      }
      Dot {
       name Dot16
       xpos -306
       ypos -192
      }
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1ba40650 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1ba48af0 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1ba48af0
    push $N1ba40650
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1ba5c520 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1ba93ca0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1ba93ca0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1ba5c520
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1950
      }
    push $N1b9d8670
    push 0
      Viewer {
       inputs 2
       frame_range 1001-1057
       input_number 1
       masking_ratio 2.35
       name Viewer1
       xpos -640
       ypos 550
      }
     end_group
    set N1b982120 [stack 0]
    push $N1b982120
     Group {
      inputs 2
      name IBKColourV3_9
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 3060
      ypos 894
      addUserKnob {20 "" l Parameters}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_1.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name matte_input
       xpos -740
       ypos -430
       number 1
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -323
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos -226
      }
      Expression {
       expr3 r+g+b
       name Expression1
       xpos -740
       ypos -54
      }
      Expression {
       expr3 a>0?1:0
       name Expression2
       xpos -740
       ypos 18
      }
    set N1bb7e300 [stack 0]
      Dot {
       name Dot1
       xpos -706
       ypos 274
      }
      Dot {
       name Dot2
       xpos 16
       ypos 274
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1bb96180 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1bb96180
      Input {
       inputs 0
       name Input1
       xpos -340
       ypos -850
      }
      Dot {
       name Dot16
       xpos -306
       ypos -192
      }
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1bbe62e0 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1bbee780 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1bbee780
    push $N1bbe62e0
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1bc02200 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1bc39950 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1bc39950
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1bc02200
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1950
      }
    push $N1bb7e300
    push 0
      Viewer {
       inputs 2
       frame_range 1001-1057
       input_number 1
       masking_ratio 2.35
       name Viewer1
       xpos -640
       ypos 550
      }
     end_group
    set N1bb27db0 [stack 0]
    push $N1bb27db0
     Group {
      inputs 2
      name IBKColourV3_10
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 3060
      ypos 954
      addUserKnob {20 "" l Parameters}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_1.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name matte_input
       xpos -740
       ypos -430
       number 1
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -323
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos -226
      }
      Expression {
       expr3 r+g+b
       name Expression1
       xpos -740
       ypos -54
      }
      Expression {
       expr3 a>0?1:0
       name Expression2
       xpos -740
       ypos 18
      }
    set N1bd23fa0 [stack 0]
      Dot {
       name Dot1
       xpos -706
       ypos 274
      }
      Dot {
       name Dot2
       xpos 16
       ypos 274
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1bd3be20 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1bd3be20
      Input {
       inputs 0
       name Input1
       xpos -340
       ypos -850
      }
      Dot {
       name Dot16
       xpos -306
       ypos -192
      }
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1bd8bf90 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1bd94440 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1bd94440
    push $N1bd8bf90
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1bda7ec0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1bddf5f0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1bddf5f0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1bda7ec0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1950
      }
    push $N1bd23fa0
    push 0
      Viewer {
       inputs 2
       frame_range 1001-1057
       input_number 1
       masking_ratio 2.35
       name Viewer1
       xpos -640
       ypos 550
      }
     end_group
    set N1bccd990 [stack 0]
    push $N1bccd990
     Group {
      inputs 2
      name IBKColourV3_11
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 3060
      ypos 1038
      addUserKnob {20 "" l Parameters}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_1.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name matte_input
       xpos -740
       ypos -430
       number 1
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -323
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos -226
      }
      Expression {
       expr3 r+g+b
       name Expression1
       xpos -740
       ypos -54
      }
      Expression {
       expr3 a>0?1:0
       name Expression2
       xpos -740
       ypos 18
      }
    set N1bec9c20 [stack 0]
      Dot {
       name Dot1
       xpos -706
       ypos 274
      }
      Dot {
       name Dot2
       xpos 16
       ypos 274
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1bee1aa0 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1bee1aa0
      Input {
       inputs 0
       name Input1
       xpos -340
       ypos -850
      }
      Dot {
       name Dot16
       xpos -306
       ypos -192
      }
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1bf31c10 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1bf3a0b0 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1bf3a0b0
    push $N1bf31c10
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1bf4db30 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1bf85260 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1bf85260
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1bf4db30
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1950
      }
    push $N1bec9c20
    push 0
      Viewer {
       inputs 2
       frame_range 1001-1057
       input_number 1
       masking_ratio 2.35
       name Viewer1
       xpos -640
       ypos 550
      }
     end_group
    set N1be73700 [stack 0]
    push $N1be73700
     Group {
      inputs 2
      name IBKColourV3_12
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 3060
      ypos 1110
      addUserKnob {20 "" l Parameters}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_1.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name matte_input
       xpos -740
       ypos -430
       number 1
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -323
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos -226
      }
      Expression {
       expr3 r+g+b
       name Expression1
       xpos -740
       ypos -54
      }
      Expression {
       expr3 a>0?1:0
       name Expression2
       xpos -740
       ypos 18
      }
    set N1c06fcb0 [stack 0]
      Dot {
       name Dot1
       xpos -706
       ypos 274
      }
      Dot {
       name Dot2
       xpos 16
       ypos 274
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1c087b30 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1c087b30
      Input {
       inputs 0
       name Input1
       xpos -340
       ypos -850
      }
      Dot {
       name Dot16
       xpos -306
       ypos -192
      }
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1c0d7c90 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1c0e0130 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1c0e0130
    push $N1c0d7c90
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1c0f3bb0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1c12b2e0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1c12b2e0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1c0f3bb0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1950
      }
    push $N1c06fcb0
    push 0
      Viewer {
       inputs 2
       frame_range 1001-1057
       input_number 1
       masking_ratio 2.35
       name Viewer1
       xpos -640
       ypos 550
      }
     end_group
    set N1c019370 [stack 0]
    push $N1c019370
     Group {
      inputs 2
      name IBKColourV3_13
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 3060
      ypos 1194
      addUserKnob {20 "" l Parameters}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_1.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name matte_input
       xpos -740
       ypos -430
       number 1
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -323
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos -226
      }
      Expression {
       expr3 r+g+b
       name Expression1
       xpos -740
       ypos -54
      }
      Expression {
       expr3 a>0?1:0
       name Expression2
       xpos -740
       ypos 18
      }
    set N1c215930 [stack 0]
      Dot {
       name Dot1
       xpos -706
       ypos 274
      }
      Dot {
       name Dot2
       xpos 16
       ypos 274
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1c22d7b0 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1c22d7b0
      Input {
       inputs 0
       name Input1
       xpos -340
       ypos -850
      }
      Dot {
       name Dot16
       xpos -306
       ypos -192
      }
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1c27d910 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1c285db0 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1c285db0
    push $N1c27d910
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1c299790 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1c2d0f50 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1c2d0f50
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1c299790
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1950
      }
    push $N1c215930
    push 0
      Viewer {
       inputs 2
       frame_range 1001-1057
       input_number 1
       masking_ratio 2.35
       name Viewer1
       xpos -640
       ypos 550
      }
     end_group
    set N1c1bf3f0 [stack 0]
    push $N1c1bf3f0
     Group {
      inputs 2
      name IBKColourV3_14
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 3060
      ypos 1278
      addUserKnob {20 "" l Parameters}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_1.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name matte_input
       xpos -740
       ypos -430
       number 1
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -323
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos -226
      }
      Expression {
       expr3 r+g+b
       name Expression1
       xpos -740
       ypos -54
      }
      Expression {
       expr3 a>0?1:0
       name Expression2
       xpos -740
       ypos 18
      }
    set N1c3bb5a0 [stack 0]
      Dot {
       name Dot1
       xpos -706
       ypos 274
      }
      Dot {
       name Dot2
       xpos 16
       ypos 274
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1c3d3420 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1c3d3420
      Input {
       inputs 0
       name Input1
       xpos -340
       ypos -850
      }
      Dot {
       name Dot16
       xpos -306
       ypos -192
      }
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1c423580 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1c42ba40 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1c42ba40
    push $N1c423580
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1c43f4c0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1c476c00 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1c476c00
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1c43f4c0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1950
      }
    push $N1c3bb5a0
    push 0
      Viewer {
       inputs 2
       frame_range 1001-1057
       input_number 1
       masking_ratio 2.35
       name Viewer1
       xpos -640
       ypos 550
      }
     end_group
    set N1c365050 [stack 0]
    push $N1c365050
     Group {
      inputs 2
      name IBKColourV3_15
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 3060
      ypos 1362
      addUserKnob {20 "" l Parameters}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_1.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off} {parent.IBKColourV3_1.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult} {parent.IBKColourV3_1.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name matte_input
       xpos -740
       ypos -430
       number 1
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -323
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos -226
      }
      Expression {
       expr3 r+g+b
       name Expression1
       xpos -740
       ypos -54
      }
      Expression {
       expr3 a>0?1:0
       name Expression2
       xpos -740
       ypos 18
      }
    set N1c567280 [stack 0]
      Dot {
       name Dot1
       xpos -706
       ypos 274
      }
      Dot {
       name Dot2
       xpos 16
       ypos 274
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1c57f100 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1c57f100
      Input {
       inputs 0
       name Input1
       xpos -340
       ypos -850
      }
      Dot {
       name Dot16
       xpos -306
       ypos -192
      }
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1c5cf260 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1c5d7700 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1c5d7700
    push $N1c5cf260
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1c5eb180 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1c6228b0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1c6228b0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1c5eb180
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1950
      }
    push $N1c567280
    push 0
      Viewer {
       inputs 2
       frame_range 1001-1057
       input_number 1
       masking_ratio 2.35
       name Viewer1
       xpos -640
       ypos 550
      }
     end_group
    push $N1c365050
    push $N1c1bf3f0
    push $N1c019370
    push $N1be73700
    push $N1bccd990
    push $N1bb27db0
    push $N1b982120
    push $N1b7dc490
    push $N1b630800
    push $N1b48ab90
    push $N1b2e8cf0
     Switch {
      inputs 12
      which {{parent.Switch1.which}}
      name Switch3
      xpos 2760
      ypos 1713
     }
     Keymix {
      inputs 3
      channels rgb
      name Keymix4
      xpos 3187
      ypos 1713
     }
     Dot {
      name Dot3
      xpos 3221
      ypos 2311
     }
    push $N1b0ba5d0
     Dot {
      name Dot39
      xpos 1861
      ypos -6
     }
    push $N1b118140
     Group {
      name IBKColourV3_7
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      selected true
      xpos 1961
      ypos -280
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_6.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      erode {{parent.IBKColourV3_6.erode}}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{parent.IBKColourV3_6.multi}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1c6f9e00 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1c715920 [stack 0]
    push $N1c715920
    push $N1c715920
      IBK {
       inputs 3
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xffff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1c73cec0 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1c73cec0
    push $N1c6f9e00
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop1
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1c7832a0 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1c78b740 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1c78b740
    push $N1c7832a0
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1c79f200 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1c7d6900 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1c7d6900
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1c79f200
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
     Merge2 {
      inputs 2
      operation stencil
      name Merge2
      xpos 1961
      ypos -10
     }
     Dot {
      name Dot43
      xpos 1995
      ypos 104
     }
    set N1c84f210 [stack 0]
     Dot {
      name Dot22
      xpos 2095
      ypos 104
     }
     Crop {
      box {0 0 {input.width} {input.height}}
      name Crop3
      xpos 2060
      ypos 1590
     }
     FilterErode {
      size 20
      filter gaussian
      name FilterErode2
      xpos 2060
      ypos 1662
     }
    push $N1b11cfe0
     Dot {
      name Dot26
      xpos 2394
      ypos 1717
     }
    push $N1c84f210
     Group {
      name IBKColourV3_18
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1961
      ypos 481
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_7.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi 1
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1c8989c0 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1c8b44f0 [stack 0]
    push $N1c8b44f0
    push $N1c8b44f0
      IBK {
       inputs 3
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1c8dba90 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1c8dba90
    push $N1c8989c0
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1c921e70 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1c92a310 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1c92a310
    push $N1c921e70
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1c93ddd0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1c9754d0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1c9754d0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1c93ddd0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1c874d30 [stack 0]
     Group {
      name IBKColourV3_19
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1961
      ypos 565
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_7.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1ca0bf60 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1ca27a90 [stack 0]
    push $N1ca27a90
    push $N1ca27a90
      IBK {
       inputs 3
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1ca4f030 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1ca4f030
    push $N1ca0bf60
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1ca95420 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1ca9d8d0 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1ca9d8d0
    push $N1ca95420
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1cab1390 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1cae8aa0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1cae8aa0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1cab1390
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1c9e80b0 [stack 0]
     Group {
      name IBKColourV3_20
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1961
      ypos 649
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_7.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1cb7f470 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1cb9b040 [stack 0]
    push $N1cb9b040
    push $N1cb9b040
      IBK {
       inputs 3
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1cbc25e0 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1cbc25e0
    push $N1cb7f470
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1cc089c0 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1cc10e60 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1cc10e60
    push $N1cc089c0
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1cc24890 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1cc5c020 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1cc5c020
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1cc24890
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1cb5b660 [stack 0]
     Group {
      name IBKColourV3_21
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1961
      ypos 733
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_7.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1ccf2aa0 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1cd0e5c0 [stack 0]
    push $N1cd0e5c0
    push $N1cd0e5c0
      IBK {
       inputs 3
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1cd35b60 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1cd35b60
    push $N1ccf2aa0
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1cd7bf50 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1cd843f0 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1cd843f0
    push $N1cd7bf50
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1cd97eb0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1cdcf5d0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1cdcf5d0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1cd97eb0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1cccebe0 [stack 0]
     Group {
      name IBKColourV3_26
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1961
      ypos 824
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_7.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1ce66040 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1ce81b60 [stack 0]
    push $N1ce81b60
    push $N1ce81b60
      IBK {
       inputs 3
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1cea9100 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1cea9100
    push $N1ce66040
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1ceef4e0 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1cef7980 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1cef7980
    push $N1ceef4e0
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1cf0b440 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1cf42b40 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1cf42b40
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1cf0b440
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1ce42190 [stack 0]
     Group {
      name IBKColourV3_27
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1961
      ypos 901
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_7.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1cfd95c0 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1cff50e0 [stack 0]
    push $N1cff50e0
    push $N1cff50e0
      IBK {
       inputs 3
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1d01c680 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1d01c680
    push $N1cfd95c0
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1d062a60 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1d06af00 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1d06af00
    push $N1d062a60
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1d07e9c0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1d0b60c0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1d0b60c0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1d07e9c0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1cfb5700 [stack 0]
     Group {
      name IBKColourV3_28
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1961
      ypos 985
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_7.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1d14caa0 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1d168670 [stack 0]
    push $N1d168670
    push $N1d168670
      IBK {
       inputs 3
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1d18fc10 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1d18fc10
    push $N1d14caa0
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1d1d5ff0 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1d1de490 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1d1de490
    push $N1d1d5ff0
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1d1f1eb0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1d229640 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1d229640
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1d1f1eb0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1d128c90 [stack 0]
     Group {
      name IBKColourV3_29
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1961
      ypos 1069
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_7.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1d2c00d0 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1d2dbbf0 [stack 0]
    push $N1d2dbbf0
    push $N1d2dbbf0
      IBK {
       inputs 3
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1d303190 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1d303190
    push $N1d2c00d0
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1d349580 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1d351a20 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1d351a20
    push $N1d349580
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1d3654e0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1d39cbe0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1d39cbe0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1d3654e0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1d29c210 [stack 0]
     Group {
      name IBKColourV3_30
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1961
      ypos 1153
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_7.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1d433670 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1d44f190 [stack 0]
    push $N1d44f190
    push $N1d44f190
      IBK {
       inputs 3
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1d476730 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1d476730
    push $N1d433670
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1d4bcb10 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1d4c4fb0 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1d4c4fb0
    push $N1d4bcb10
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1d4d8a70 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1d510170 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1d510170
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1d4d8a70
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1d40f7a0 [stack 0]
     Group {
      name IBKColourV3_43
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1961
      ypos 1237
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_7.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1d5a6be0 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1d5c2700 [stack 0]
    push $N1d5c2700
    push $N1d5c2700
      IBK {
       inputs 3
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1d5e9ca0 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1d5e9ca0
    push $N1d5a6be0
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1d630080 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1d638520 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1d638520
    push $N1d630080
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1d64bfe0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1d6836e0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1d6836e0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1d64bfe0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1d582d30 [stack 0]
     Group {
      name IBKColourV3_44
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1961
      ypos 1321
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_7.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1d71a0c0 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1d735c90 [stack 0]
    push $N1d735c90
    push $N1d735c90
      IBK {
       inputs 3
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1d75d230 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1d75d230
    push $N1d71a0c0
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1d7a3610 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1d7abab0 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1d7abab0
    push $N1d7a3610
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1d7bf4d0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1d7f6c60 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1d7f6c60
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1d7bf4d0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1d6f62a0 [stack 0]
     Group {
      name IBKColourV3_45
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1961
      ypos 1427
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_7.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off} {parent.IBKColourV3_7.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult} {parent.IBKColourV3_7.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1d88d710 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1d8a9230 [stack 0]
    push $N1d8a9230
    push $N1d8a9230
      IBK {
       inputs 3
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1d8d07d0 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1d8d07d0
    push $N1d88d710
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1d916bc0 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1d91f060 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1d91f060
    push $N1d916bc0
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1d932b20 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1d96a220 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1d96a220
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1d932b20
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    push $N1d6f62a0
    push $N1d582d30
    push $N1d40f7a0
    push $N1d29c210
    push $N1d128c90
    push $N1cfb5700
    push $N1ce42190
    push $N1cccebe0
    push $N1cb5b660
    push $N1c9e80b0
    push $N1c874d30
     Switch {
      inputs 12
      which {{parent.Switch1.which}}
      name Switch2
      xpos 1660
      ypos 1713
     }
     Keymix {
      inputs 3
      channels rgb
      name Keymix1
      xpos 2060
      ypos 1713
     }
     Dot {
      name Dot2
      xpos 2094
      ypos 1857
     }
    push $N1b0b5880
     Dot {
      name Dot35
      xpos 933
      ypos -26
     }
    push $N1b118140
     Dot {
      name Dot21
      xpos 1394
      ypos -665
     }
    set N1d9f8530 [stack 0]
     Dot {
      name Dot20
      xpos 1067
      ypos -665
     }
    set N1d9fd440 [stack 0]
     Group {
      name IBKColourV3_6
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1033
      ypos -265
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size 1
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.darks} {parent.darks} {parent.darks}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.lights} {parent.lights} {parent.lights}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      erode {{parent.erode}}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1da25f80 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1da41ac0 [stack 0]
    push $N1da41ac0
    push $N1da41ac0
      IBK {
       inputs 3
       screen_type green
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1da69050 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1da69050
    push $N1da25f80
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop1
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1daaf420 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1dab78c0 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1dab78c0
    push $N1daaf420
      Dot {
       name Dot4
       xpos 269
       ypos 857
      }
    set N1dacb380 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1db02a80 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1db02a80
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1dacb380
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
     Merge2 {
      inputs 2
      operation stencil
      name Merge176
      xpos 1033
      ypos -30
     }
     Dot {
      name Dot42
      xpos 1067
      ypos 134
     }
    set N1db7b350 [stack 0]
     Dot {
      name Dot30
      xpos 1194
      ypos 134
     }
     Crop {
      box {0 0 {input.width} {input.height}}
      name Crop2
      xpos 1160
      ypos 1590
     }
     FilterErode {
      size 20
      filter gaussian
      name FilterErode1
      xpos 1160
      ypos 1629
     }
    push $N1d9f8530
     Dot {
      name Dot31
      xpos 1394
      ypos 1717
     }
    push $N1db7b350
     Group {
      name IBKColourV3_31
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1033
      ypos 494
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_6.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi 1
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1dbc4900 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1dbe0420 [stack 0]
    push $N1dbe0420
    push $N1dbe0420
      IBK {
       inputs 3
       screen_type green
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1dc079b0 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1dc079b0
    push $N1dbc4900
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1dc4dd90 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1dc56230 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1dc56230
    push $N1dc4dd90
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1dc69cf0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1dca13f0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1dca13f0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1dc69cf0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1dba0c70 [stack 0]
     Group {
      name IBKColourV3_32
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1033
      ypos 578
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_6.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1dd3de80 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1dd599a0 [stack 0]
    push $N1dd599a0
    push $N1dd599a0
      IBK {
       inputs 3
       screen_type green
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1dd80f30 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1dd80f30
    push $N1dd3de80
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1ddc7320 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1ddcf7c0 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1ddcf7c0
    push $N1ddc7320
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1dde3280 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1de1a980 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1de1a980
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1dde3280
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1dd13fb0 [stack 0]
     Group {
      name IBKColourV3_33
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1033
      ypos 662
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_6.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1deb1400 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1deccf20 [stack 0]
    push $N1deccf20
    push $N1deccf20
      IBK {
       inputs 3
       screen_type green
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1def44b0 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1def44b0
    push $N1deb1400
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1df3a890 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1df42d30 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1df42d30
    push $N1df3a890
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1df567f0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1df8df00 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1df8df00
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1df567f0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1de8d540 [stack 0]
     Group {
      name IBKColourV3_34
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1033
      ypos 746
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_6.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1e024980 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1e0404a0 [stack 0]
    push $N1e0404a0
    push $N1e0404a0
      IBK {
       inputs 3
       screen_type green
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1e067a30 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1e067a30
    push $N1e024980
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1e0ade10 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1e0b62b0 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1e0b62b0
    push $N1e0ade10
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1e0c9d70 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1e101470 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1e101470
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1e0c9d70
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1e000a70 [stack 0]
     Group {
      name IBKColourV3_35
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1033
      ypos 830
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_6.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1e1987f0 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1e1b4310 [stack 0]
    push $N1e1b4310
    push $N1e1b4310
      IBK {
       inputs 3
       screen_type green
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1e1db8a0 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1e1db8a0
    push $N1e1987f0
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1e221c90 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1e22a130 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1e22a130
    push $N1e221c90
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1e23dbf0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1e2752f0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1e2752f0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1e23dbf0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1e174030 [stack 0]
     Group {
      name IBKColourV3_36
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1033
      ypos 914
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_6.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1e30bd70 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1e3278b0 [stack 0]
    push $N1e3278b0
    push $N1e3278b0
      IBK {
       inputs 3
       screen_type green
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1e34ee40 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1e34ee40
    push $N1e30bd70
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1e395230 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1e39d6d0 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1e39d6d0
    push $N1e395230
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1e3b1190 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1e3e88d0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1e3e88d0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1e3b1190
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1e2e7eb0 [stack 0]
     Group {
      name IBKColourV3_37
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1033
      ypos 998
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_6.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1e47f340 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1e49ae60 [stack 0]
    push $N1e49ae60
    push $N1e49ae60
      IBK {
       inputs 3
       screen_type green
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1e4c23f0 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1e4c23f0
    push $N1e47f340
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1e5087d0 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1e510c70 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1e510c70
    push $N1e5087d0
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1e524730 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1e55be30 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1e55be30
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1e524730
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1e45b490 [stack 0]
     Group {
      name IBKColourV3_38
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1033
      ypos 1082
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_6.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1e5f28b0 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1e60e3d0 [stack 0]
    push $N1e60e3d0
    push $N1e60e3d0
      IBK {
       inputs 3
       screen_type green
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1e635960 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1e635960
    push $N1e5f28b0
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1e67bd40 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1e6841e0 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1e6841e0
    push $N1e67bd40
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1e697ca0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1e6cf3a0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1e6cf3a0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1e697ca0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1e5ce9f0 [stack 0]
     Group {
      name IBKColourV3_39
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1033
      ypos 1166
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_6.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1e765e30 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1e781950 [stack 0]
    push $N1e781950
    push $N1e781950
      IBK {
       inputs 3
       screen_type green
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1e7a8ee0 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1e7a8ee0
    push $N1e765e30
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1e7ef2d0 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1e7f7770 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1e7f7770
    push $N1e7ef2d0
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1e80b230 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1e842930 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1e842930
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1e80b230
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1e741f70 [stack 0]
     Group {
      name IBKColourV3_40
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1033
      ypos 1250
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_6.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1e8d93a0 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1e8f4ee0 [stack 0]
    push $N1e8f4ee0
    push $N1e8f4ee0
      IBK {
       inputs 3
       screen_type green
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1e91c470 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1e91c470
    push $N1e8d93a0
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1e962860 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1e96ad10 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1e96ad10
    push $N1e962860
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1e97e7d0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1e9b5ee0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1e9b5ee0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1e97e7d0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1e8b54f0 [stack 0]
     Group {
      name IBKColourV3_41
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1033
      ypos 1334
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_6.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1ea4c970 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1ea68490 [stack 0]
    push $N1ea68490
    push $N1ea68490
      IBK {
       inputs 3
       screen_type green
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1ea8fa20 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1ea8fa20
    push $N1ea4c970
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1ead5e00 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1eade2a0 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1eade2a0
    push $N1ead5e00
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1eaf1d60 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1eb29460 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1eb29460
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1eaf1d60
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    set N1ea28ab0 [stack 0]
     Group {
      name IBKColourV3_42
      help "This node provides IBKGizmo a colour reference in which to base its keying algorithm on a per pixel basis.\nThe idea is to remove the foreground image and only leave the shades and hues of the original blue/greenscreen.\nAttach the output of this node to the 'c' input of a default IBKGizmo. Attach the input of this node along with the 'fg' input of the IBKGizmo to the original screen.\nPick which colour your screen type is in both nodes and then while viewing the alpha output from the IBKGizmo lower the darks.b (if a bluescreen - adjust darks.g if a greenscreen) in this node \nuntil you see a change in the garbage area of the matte. Once you see a change then you have gone too far -back off a step. If you are still left with discoloured edges you can use the other colours in the lights and darks to eliminate them. Remember the idea is \nto be left with the original shades of the screen and the foreground blacked out. While swapping between viewing the matte from the IBKGizmo and the rgb output of this IBKColour adjust the other colours \nuntil you see a change in the garbage area of the matte. Simple rule of thumb - if you have a light red discoloured area increase the lights.r - if you have a dark green discoloured area increase darks.g. If your screen does not have a very saturated hue you may still be left\n with areas of discolouration after the above process. The 'erode' slider can help with this - while viewing the rgb output adjust the erode until those areas disappear.\nThe 'patch black' slider allows you to fill in the black areas with screen colour. This is not always necessary but if you see blue squares in your composite increase this value and it'll fix it.\nThe above is the only real workflow for this node - working from the top parameter to the bottom parameter- going back to tweak darks/lights with 'erode' and 'patch black' activated isn't really gonna work. "
      tile_color 0x990000
      label MODIFIED
      xpos 1033
      ypos 1418
      addUserKnob {20 "" l Parameters}
      addUserKnob {41 screen_type l "screen type" T IBK2.screen_type}
      addUserKnob {16 Size l size t "size of colour expansion" R 0 100}
      Size {{parent.IBKColourV3_6.Size}}
      addUserKnob {18 off l darks t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R -1 1}
      off {{parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off} {parent.IBKColourV3_6.off}}
      addUserKnob {6 off_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 off_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {18 mult l lights t "adjust the colour values to get the best separation between black and the screen type colour.\nYou want to be left with only shades of the screen colour and black. \nIf a green screen is selected start by bringing down darks->green\nIf a blue screen is selected start by bringing down darks->blue" R 0 2}
      mult {{parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult} {parent.IBKColourV3_6.mult}}
      addUserKnob {6 mult_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 mult_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {7 erode t "increase this value if you still see traces of the foreground edge colour in the output" R 0 5}
      addUserKnob {26 ""}
      addUserKnob {7 multi l "patch black" t "increase this to optionally remove the black from the output.\nThis should only be used once the the above darks/lights have been set" R 0 5}
      multi {{input.multi*2}}
      addUserKnob {6 filt l INVISIBLE -STARTLINE +INVISIBLE}
      filt true
      addUserKnob {26 ""}
      addUserKnob {7 level l INVISIBLE t "multiply the rgb output. Helps remove noise from main key" +INVISIBLE}
      level 1
     }
      Input {
       inputs 0
       name Input1
       xpos -31
       ypos -430
      }
      Dot {
       name Dot16
       xpos 3
       ypos -196
      }
    set N1ebbfee0 [stack 0]
      Dot {
       name Dot1
       tile_color 0x9597bf00
       xpos -706
       ypos -196
      }
      Grade {
       multiply {{mult.r} {mult.g} {mult.b} {curve}}
       add {{off.r} {off.g} {off.b} {curve}}
       name Grade11
       tile_color 0x7aa9ff00
       xpos -740
       ypos -80
      }
      Clamp {
       maximum_enable false
       name Clamp2
       xpos -740
       ypos 17
      }
    set N1ebdba00 [stack 0]
    push $N1ebdba00
    push $N1ebdba00
      IBK {
       inputs 3
       screen_type green
       blue_green_weight 1
       luma 1
       name IBK2
       tile_color 0xff00ff
       label "\[value screen_type]"
       xpos -740
       ypos 264
      }
      Invert {
       channels alpha
       name Invert1
       tile_color 0x7aa9ff00
       xpos -18
       ypos 264
      }
      Erode {
       size {{erode}}
       name Erode1
       xpos -18
       ypos 404
      }
    set N1ec02f90 [stack 0]
      Dot {
       name Dot3
       tile_color 0x9597bf00
       xpos 16
       ypos 1534
      }
    push $N1ec02f90
    push $N1ebbfee0
      Dot {
       name Dot17
       xpos 269
       ypos -192
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       channels alpha
       name Copy3
       tile_color 0x9e3c6300
       xpos 235
       ypos 398
      }
      Crop {
       box {{curve} {curve} {input.width} {input.height}}
       reformat true
       crop false
       name Crop2
       selected true
       xpos 235
       ypos 494
      }
      Premult {
       name Premult3
       xpos 235
       ypos 550
      }
      Blur {
       size {{Size}}
       name Blur4
       tile_color 0xcc804e00
       xpos 235
       ypos 614
      }
      Unpremult {
       name Unpremult4
       xpos 235
       ypos 724
      }
    set N1ec49370 [stack 0]
      Clamp {
       channels {rgba.red rgba.green rgba.blue -rgba.alpha}
       maximum 0
       MinClampTo_enable true
       MaxClampTo_enable true
       name Clamp1
       xpos 355
       ypos 724
      }
    set N1ec51810 [stack 0]
      Dot {
       name Dot5
       xpos 594
       ypos 728
      }
    push $N1ec51810
    push $N1ec49370
      Dot {
       name Dot4
       xpos 269
       ypos 859
      }
    set N1ec652d0 [stack 0]
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy1
       xpos 355
       ypos 848
      }
      Blur {
       channels rgba
       size {{Size*3*multi}}
       name Blur1
       xpos 355
       ypos 932
      }
      Unpremult {
       name Unpremult1
       xpos 355
       ypos 992
      }
      Copy {
       inputs 2
       from0 rgba.red
       to0 rgba.alpha
       name Copy2
       xpos 560
       ypos 985
      }
      Invert {
       channels alpha
       name Invert2
       xpos 560
       ypos 1157
      }
    set N1ec9c9d0 [stack 0]
      FilterErode {
       channels alpha
       size {{(-Size/5)}}
       filter gaussian
       name FilterErode2
       xpos 560
       ypos 1244
      }
    push $N1ec9c9d0
      FilterErode {
       channels alpha
       size {{(-Size/5)*multi*2}}
       filter gaussian
       name FilterErode1
       xpos 358
       ypos 1157
      }
      Switch {
       inputs 2
       which {{1-filt}}
       name Switch1
       xpos 358
       ypos 1251
      }
      Premult {
       name Premult1
       xpos 358
       ypos 1390
      }
    push $N1ec652d0
      Merge {
       inputs 2
       name Merge1
       xpos 233
       ypos 1390
      }
      Copy {
       inputs 2
       from0 rgba.alpha
       to0 rgba.alpha
       name ChannelCopy2
       tile_color 0x9e3c6300
       xpos 233
       ypos 1524
      }
      Grade {
       multiply {{level}}
       name Grade1
       xpos 233
       ypos 1633
      }
      Crop {
       box {0 0 {input.width} {input.height}}
       name Crop1
       xpos 233
       ypos 1685
      }
      Output {
       name Output1
       xpos 233
       ypos 1810
      }
     end_group
    push $N1ea28ab0
    push $N1e8b54f0
    push $N1e741f70
    push $N1e5ce9f0
    push $N1e45b490
    push $N1e2e7eb0
    push $N1e174030
    push $N1e000a70
    push $N1de8d540
    push $N1dd13fb0
    push $N1dba0c70
     Switch {
      inputs 12
      which {{parent.cleaning_iterations}}
      name Switch1
      xpos 701
      ypos 1713
     }
     Keymix {
      inputs 3
      channels rgb
      name Keymix3
      xpos 1160
      ypos 1713
     }
     Dot {
      name Dot4
      xpos 1194
      ypos 2067
     }
     Switch {
      inputs 2
      which {{parent.screen_type}}
      name Switch4
      xpos 2060
      ypos 2063
     }
     Switch {
      inputs 2
      which {{parent.matte_input}}
      name Switch12
      xpos 2060
      ypos 2307
     }
     Switch {
      inputs 2
      which {{parent.clean_plate_input}}
      name Switch8
      xpos 2060
      ypos 2577
     }
     Dot {
      name Dot816
      xpos 2094
      ypos 2837
     }
    set N1ed3e880 [stack 0]
     Dot {
      name Dot15
      xpos 2694
      ypos 2837
     }
     Dot {
      name Dot16
      xpos 2694
      ypos 4957
     }
     Dot {
      name Dot45
      xpos 2294
      ypos 4956
     }
    set N1ed4d5d0 [stack 0]
    push $N1ed3e880
     Dot {
      name Dot11
      xpos 2094
      ypos 3047
     }
    set N1ed524e0 [stack 0]
    push $N1d9fd440
     Dot {
      name Dot19
      xpos 494
      ypos -665
     }
     Dot {
      name Dot9
      xpos 40
      ypos -665
     }
     Dot {
      name Dot18
      xpos 40
      ypos 2277
     }
     Dot {
      name Dot10
      xpos 40
      ypos 2837
     }
    set N1ed66090 [stack 0]
     Dot {
      name Dot813
      xpos 1094
      ypos 2837
     }
    set N1ed6b030 [stack 0]
     Dot {
      name Dot7
      xpos 1094
      ypos 2663
     }
     Dot {
      name Dot6
      xpos 1494
      ypos 2663
     }
     Group {
      inputs 2
      name IBKGizmoV3_2
      help "There are 2 basic approaches to pull a key with IBK. One is to use IBKColour and the C input and the other is to pick a colour which best represents the area you are trying to key.\n\nThe colour weights deal with the hardness of the matte. If you view the output (with screen subtraction ticked on) you will typically see areas where edges have a slight discolouration\ndue to the background not being fully removed from the original plate. This is not spill but a result of the matte being too strong. Lowering one of the weights will correct that particular edge. If it's\na red foreground image with an edge problem bring down the red weight - same idea for the other weight. This may affect other edges so the use of multiple IBKGizmo's with different weights split with KeyMixes is recommended.\n\nThe 'luminance match' feature adds a luminance factor to the keying algorithm which helps to capture transparent areas of the foreground which are brighter than the backing screen. It will also allow you to lessen some of the garbage area noise by bringing down the screen range - \npushing this control too far will also eat into some of your foreground blacks. 'Luminance level' allows you to make the overall effect stronger or weaker.\n\n'autolevels' will perform a colour correction before the image is pulled so that hard edges from a foreground subject with saturated colours are reduced. The same can be achieved with the weights but here only those saturated colours are affected whereas the use of weights will\naffect the entire image. When using this feature it's best to have this as a separate node which you can then split with other IBKGizmos as the weights will no longer work as expected. You can override some of the logic for when you actually have particular foreground colours you want to keep.\nFor example when you have a saturated red subject against bluescreen you'll get a magenta transition area. Autolevels will eliminate this but if you have a magenta foreground object then this control will make the magenta more red unless you check the magenta box to keep it.\n\n'screen subtraction' will remove the backing from the rgb via a subtraction process. Unchecking this will simply premultiply the original fg with the generated matte.\n\n'use bkg luminance' and 'use bkg chroma' allow you to affect the output rgb by the new bg. These controls are best used with the 'luminance match' sliders above. This feature can also sometimes really help with screens that exhibit some form of fringing artifact - usually a darkening or\nlightening of an edge on one of the colour channels on the screen. You can offset the effect by grading the bg input up or down with a grade node just before input. If it's just an area which needs help then just bezier that area and locally grade the\nbg input  up or down to remove the artifact."
      tile_color 0x990000
      label "\[value st]"
      xpos 1463
      ypos 3037
      addUserKnob {20 "" l IBK}
      addUserKnob {4 st l "screen type" t "use the C input  to define the screen colour on a per pixel basis or use 'pick' and the colour tab below" M {C-blue C-green pick}}
      addUserKnob {18 colour l "colour " t "use this colour instead of the C input when 'pick' is enabled above"}
      colour {0 0 1}
      addUserKnob {6 colour_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {41 red_weight l "red weight" T IBK1.red_weight}
      addUserKnob {41 blue_green_weight l "blue/green weight" T IBK1.blue_green_weight}
      addUserKnob {26 ""}
      addUserKnob {41 lm_enable l "luminance match" T IBK1.lm_enable}
      addUserKnob {41 level l "screen range" T IBK1.level}
      addUserKnob {41 luma l "luminance level" T IBK1.luma}
      addUserKnob {41 ll_enable l enable T IBK1.ll_enable}
      addUserKnob {26 ""}
      addUserKnob {41 autolevels T IBK1.autolevels}
      addUserKnob {41 yellow T IBK1.yellow}
      addUserKnob {41 cyan T IBK1.cyan}
      addUserKnob {41 magenta T IBK1.magenta}
      addUserKnob {26 ""}
      addUserKnob {41 ss l "screen subtraction" T IBK1.ss}
      addUserKnob {6 ublu l "use bkg luminance" t "have the brightness of the edge be affected by the brightness of the bg input" -STARTLINE}
      addUserKnob {6 ubcr l "use bkg chroma" t "have the colour of the edge be affected by the colour of the bg input" -STARTLINE}
     }
      Input {
       inputs 0
       name bg
       xpos 559
       ypos 175
       number 2
      }
    set N1ed8fa20 [stack 0]
      Input {
       inputs 0
       name fg
       xpos 48
       ypos 178
      }
    set N1ed947d0 [stack 0]
      Grade {
       multiply 0
       add {{colour.r i} {colour.g i} {colour.b i} {curve}}
       black_clamp false
       name Grade1
       xpos 158
       ypos 178
      }
      Input {
       inputs 0
       name c
       xpos 423
       ypos 178
       number 1
      }
      Switch {
       inputs 2
       which {{st==2}}
       name Switch1
       xpos 293
       ypos 178
      }
    set N1edaf890 [stack 0]
    push $N1ed947d0
    push $N1ed947d0
      IBK {
       inputs 4
       screen_type green
       red_weight {{IBK1.red_weight}}
       blue_green_weight {{IBK1.blue_green_weight}}
       lm_enable {{IBK1.lm_enable}}
       level {{IBK1.level}}
       luma {{IBK1.luma}}
       ll_enable {{IBK1.ll_enable}}
       autolevels {{IBK1.autolevels}}
       yellow {{IBK1.yellow}}
       cyan {{IBK1.cyan}}
       magenta {{IBK1.magenta}}
       ss {{IBK1.ss}}
       clamp_alpha {{IBK1.clamp_alpha}}
       rgbal {{IBK1.rgbal}}
       ubl {{IBK1.ubl}}
       ubc {{IBK1.ubc}}
       name IBK3
       note_font_color 0xff0000
       xpos 48
       ypos 306
      }
    push $N1ed8fa20
    push $N1edaf890
    push $N1ed947d0
    push $N1ed947d0
      IBK {
       inputs 4
       red_weight 0.5
       blue_green_weight 0.5
       luma 0
       rgbal true
       ubl {{ublu}}
       ubc {{ubcr}}
       name IBK1
       note_font_color 0xff00
       xpos 293
       ypos 302
      }
      Switch {
       inputs 2
       which {{st==2?colour.g>colour.b:st}}
       name Switch6
       xpos 48
       ypos 396
      }
      Output {
       name Output1
       xpos 48
       ypos 466
      }
     end_group
    push $N1ed3e880
    push $N1ed6b030
     Group {
      inputs 2
      name IBKGizmoV3_1
      help "There are 2 basic approaches to pull a key with IBK. One is to use IBKColour and the C input and the other is to pick a colour which best represents the area you are trying to key.\n\nThe colour weights deal with the hardness of the matte. If you view the output (with screen subtraction ticked on) you will typically see areas where edges have a slight discolouration\ndue to the background not being fully removed from the original plate. This is not spill but a result of the matte being too strong. Lowering one of the weights will correct that particular edge. If it's\na red foreground image with an edge problem bring down the red weight - same idea for the other weight. This may affect other edges so the use of multiple IBKGizmo's with different weights split with KeyMixes is recommended.\n\nThe 'luminance match' feature adds a luminance factor to the keying algorithm which helps to capture transparent areas of the foreground which are brighter than the backing screen. It will also allow you to lessen some of the garbage area noise by bringing down the screen range - \npushing this control too far will also eat into some of your foreground blacks. 'Luminance level' allows you to make the overall effect stronger or weaker.\n\n'autolevels' will perform a colour correction before the image is pulled so that hard edges from a foreground subject with saturated colours are reduced. The same can be achieved with the weights but here only those saturated colours are affected whereas the use of weights will\naffect the entire image. When using this feature it's best to have this as a separate node which you can then split with other IBKGizmos as the weights will no longer work as expected. You can override some of the logic for when you actually have particular foreground colours you want to keep.\nFor example when you have a saturated red subject against bluescreen you'll get a magenta transition area. Autolevels will eliminate this but if you have a magenta foreground object then this control will make the magenta more red unless you check the magenta box to keep it.\n\n'screen subtraction' will remove the backing from the rgb via a subtraction process. Unchecking this will simply premultiply the original fg with the generated matte.\n\n'use bkg luminance' and 'use bkg chroma' allow you to affect the output rgb by the new bg. These controls are best used with the 'luminance match' sliders above. This feature can also sometimes really help with screens that exhibit some form of fringing artifact - usually a darkening or\nlightening of an edge on one of the colour channels on the screen. You can offset the effect by grading the bg input up or down with a grade node just before input. If it's just an area which needs help then just bezier that area and locally grade the\nbg input  up or down to remove the artifact."
      tile_color 0x990000
      label "\[value st]"
      xpos 1260
      ypos 2827
      addUserKnob {20 "" l IBK}
      addUserKnob {4 st l "screen type" t "use the C input  to define the screen colour on a per pixel basis or use 'pick' and the colour tab below" M {C-blue C-green pick}}
      st C-green
      addUserKnob {18 colour l "colour " t "use this colour instead of the C input when 'pick' is enabled above"}
      colour {0 0 1}
      addUserKnob {6 colour_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {6 colour_panelDropped_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1 l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {41 red_weight l "red weight" T IBK1.red_weight}
      addUserKnob {41 blue_green_weight l "blue/green weight" T IBK1.blue_green_weight}
      addUserKnob {26 ""}
      addUserKnob {41 lm_enable l "luminance match" T IBK1.lm_enable}
      addUserKnob {41 level l "screen range" T IBK1.level}
      addUserKnob {41 luma l "luminance level" T IBK1.luma}
      addUserKnob {41 ll_enable l enable T IBK1.ll_enable}
      addUserKnob {26 ""}
      addUserKnob {41 autolevels T IBK1.autolevels}
      addUserKnob {41 yellow T IBK1.yellow}
      addUserKnob {41 cyan T IBK1.cyan}
      addUserKnob {41 magenta T IBK1.magenta}
      addUserKnob {26 ""}
      addUserKnob {41 ss l "screen subtraction" T IBK1.ss}
      addUserKnob {6 ublu l "use bkg luminance" t "have the brightness of the edge be affected by the brightness of the bg input" -STARTLINE}
      addUserKnob {6 ubcr l "use bkg chroma" t "have the colour of the edge be affected by the colour of the bg input" -STARTLINE}
     }
      Input {
       inputs 0
       name bg
       xpos 559
       ypos 175
       number 2
      }
    set N1edf6ef0 [stack 0]
      Input {
       inputs 0
       name fg
       xpos 48
       ypos 178
      }
    set N1edfbca0 [stack 0]
      Grade {
       multiply 0
       add {{colour.r i} {colour.g i} {colour.b i} {curve}}
       black_clamp false
       name Grade1
       xpos 158
       ypos 178
      }
      Input {
       inputs 0
       name c
       xpos 423
       ypos 178
       number 1
      }
      Switch {
       inputs 2
       which {{st==2}}
       name Switch1
       xpos 293
       ypos 178
      }
    set N1ee16d60 [stack 0]
    push $N1edfbca0
    push $N1edfbca0
      IBK {
       inputs 4
       screen_type green
       red_weight {{IBK1.red_weight}}
       blue_green_weight {{IBK1.blue_green_weight}}
       lm_enable {{IBK1.lm_enable}}
       level {{IBK1.level}}
       luma {{IBK1.luma}}
       ll_enable {{IBK1.ll_enable}}
       autolevels {{IBK1.autolevels}}
       yellow {{IBK1.yellow}}
       cyan {{IBK1.cyan}}
       magenta {{IBK1.magenta}}
       ss {{IBK1.ss}}
       clamp_alpha {{IBK1.clamp_alpha}}
       rgbal {{IBK1.rgbal}}
       ubl {{IBK1.ubl}}
       ubc {{IBK1.ubc}}
       name IBK3
       note_font_color 0xff0000
       xpos 48
       ypos 306
      }
    push $N1edf6ef0
    push $N1ee16d60
    push $N1edfbca0
    push $N1edfbca0
      IBK {
       inputs 4
       red_weight 0.5
       blue_green_weight 0.5
       luma 0
       rgbal true
       ubl {{ublu}}
       ubc {{ubcr}}
       name IBK1
       note_font_color 0xff00
       xpos 293
       ypos 302
      }
      Switch {
       inputs 2
       which {{st==2?colour.g>colour.b:st}}
       name Switch6
       xpos 48
       ypos 396
      }
      Output {
       name Output1
       xpos 48
       ypos 466
      }
     end_group
     Switch {
      inputs 2
      which {{parent.Switch4.which}}
      name Switch5
      xpos 1260
      ypos 3043
     }
     Grade {
      channels alpha
      blackpoint {{parent.matte_blacks}}
      whitepoint {{parent.matte_whites}}
      white_clamp true
      name Grade1
      xpos 1260
      ypos 3432
     }
     Shuffle {
      in alpha
      name Shuffle70
      label "\[value in]"
      xpos 1260
      ypos 3618
     }
     Dot {
      name Dot817
      xpos 1294
      ypos 4047
     }
    set N1ee722e0 [stack 0]
     Dot {
      name Dot17
      xpos 1294
      ypos 4957
     }
     Shuffle {
      green red
      blue red
      alpha red
      name Shuffle1
      label R
      xpos 1444
      ypos 4941
     }
    push $N1ed66090
     Dot {
      name Dot14
      xpos 40
      ypos 4537
     }
     Dot {
      name Dot27
      xpos 1494
      ypos 4537
     }
     Constant {
      inputs 0
      channels rgb
      color {{parent.color_sample} {parent.color_sample} {parent.color_sample} {parent.color_sample}}
      name Constant1
      xpos 2360
      ypos 3509
     }
     Reformat {
      type "to box"
      box_width {{parent.Crop5.box.r}}
      box_height {{parent.Crop5.box.t}}
      box_fixed true
      resize distort
      pbb true
      name Reformat2
      xpos 2360
      ypos 3607
     }
     Constant {
      inputs 0
      channels rgb
      color {{parent.CurveTool14.intensitydata.r} {parent.CurveTool14.intensitydata.g} {parent.CurveTool14.intensitydata.b} 0}
      name Constant76
      xpos 2219
      ypos 3509
     }
     Reformat {
      type "to box"
      box_width {{parent.Crop5.box.r}}
      box_height {{parent.Crop5.box.t}}
      box_fixed true
      resize distort
      pbb true
      name Reformat1
      xpos 2219
      ypos 3607
     }
     Switch {
      inputs 2
      which {{parent.manual}}
      name Switch9
      xpos 2285
      ypos 3741
     }
    push $N1ed524e0
     Dot {
      name Dot818
      xpos 2094
      ypos 3444
     }
    set N1eed1510 [stack 0]
     Merge2 {
      inputs 2
      operation minus
      name Merge183
      xpos 2057
      ypos 3741
     }
     Dot {
      name Dot12
      xpos 2094
      ypos 3944
     }
    set N1eee6bd0 [stack 0]
    push $N1b0f1b20
     PostageStamp {
      name from_matte_input
      xpos 1560
      ypos 3867
      hide_input true
     }
    push $N1ee722e0
     Switch {
      inputs 2
      which {{parent.matte_input}}
      name Switch11
      xpos 1560
      ypos 4041
     }
    push $N1eee6bd0
     Dot {
      name Dot13
      xpos 2275
      ypos 3944
     }
     Merge2 {
      inputs 2
      operation stencil
      name Merge184
      xpos 2241
      ypos 4041
     }
     Dot {
      name Dot28
      xpos 2275
      ypos 4186
     }
     Switch {
      inputs 2
      which {{"\[value parent.Switch4.which]==0||\[value parent.Switch4.which]==1?0:1"}}
      name Switch7
      xpos 2060
      ypos 4183
     }
     Merge2 {
      inputs 2
      operation plus
      name Merge188
      xpos 2060
      ypos 4533
     }
    push $N1ed4d5d0
     Switch {
      inputs 3
      which {{parent.viewer}}
      name Switch6
      xpos 2060
      ypos 4953
     }
     Switch {
      inputs 2
      which {{parent.show_clean_plate}}
      name Switch10
      xpos 2060
      ypos 5069
     }
     Output {
      name Output1
      xpos 2060
      ypos 5191
     }
    push $N1eed1510
     Dot {
      name Dot8
      xpos 2142
      ypos 3444
     }
    set N1ef44620 [stack 0]
     CurveTool {
      avgframes 1
      ROI {0 0 {input.width} {input.height}}
      autocropdata {{parent.CurveTool14.ROI.x} {parent.CurveTool14.ROI.y} {parent.CurveTool14.ROI.r} {curve}}
      intensitydata {0 0 0 0}
      name CurveTool14
      xpos 2260
      ypos 3441
     }
    push $N1ef44620
     Crop {
      box {0 0 {input.width} {input.height}}
      name Crop5
      xpos 2108
      ypos 3591
     }
     StickyNote {
      inputs 0
      name _StickyNote_1
      label 3060.0,-1486.0
      xpos 4560
      ypos -9
     }
     StickyNote {
      inputs 0
      name _StickyNote_2
      label 2310.0,2577.0
      xpos 2260
      ypos -9
     }
    end_group
```
