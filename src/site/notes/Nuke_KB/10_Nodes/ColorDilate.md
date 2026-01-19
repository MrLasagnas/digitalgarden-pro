---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/color-dilate/","tags":["nuke","node","edges","color"]}
---


# ColorDilate

## 🧠 Description
Edge color dilate.

## 🎯 When to use


## ⚠️ Known pitfalls


## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code
```nuke
    set cut_paste_input [stack 0]
    version 13.2 v8
    push $cut_paste_input
    Group {
    name ColourDilate_FS1
    help http://intranet/depts/Compositing:Nuke:Gizmos:ColourDilate_FS
    knobChanged "\ntn = nuke.thisNode()\ntk = nuke.thisKnob()\n\nfullList =  tn\['fullList'].value().strip(\"\[']\").split(\"', '\")\nnodeList = fullList\[0], fullList\[1], fullList\[2], fullList\[3], fullList\[4], fullList\[5]\npreviousLoopNum = tn\['previousLoopNum'].getValue()\n\ngrowTexture = tn\['growTexture'].getValue()\nqualityVal = int( tn\['quality'].getValue() )\n\n\n##### DEFINE VALUE CHANGES ##################\n\ndef valChange():\n    qualityBias = tn\['qualityBias'].getValue()\n    for i in range (0, qualityVal):\n        blurVal = nuke.toNode(fullList\[i*len(nodeList)])\n        erodeVal = nuke.toNode(fullList\[i*len(nodeList)+3])\n        if isinstance(growTexture, float) == True:\n            blurVal\['size'].setValue( (growTexture/qualityVal*(i+1)*(1-qualityBias)) + (growTexture/qualityVal*(i+1)) * (qualityBias*(pow(i+1, 2)/pow(qualityVal, 2))) )\n            erodeVal\['size'].setValue(blurVal\['size'].getValue()/2)\n        else:\n            blurVal\['size'].setValue(\[ (growTexture\[0]/qualityVal*(i+1)*(1-qualityBias)) + (growTexture\[0]/qualityVal*(i+1)) * (qualityBias*(pow(i+1, 2)/pow(qualityVal, 2))) , (growTexture\[1]/qualityVal*(i+1)*(1-qualityBias)) + (growTexture\[1]/qualityVal*(i+1)) * (qualityBias*(pow(i+1, 2)/pow(qualityVal, 2))) ])\n            erodeVal\['size'].setValue(\[blurVal\['size'].getValue()\[0]/2,blurVal\['size'].getValue()\[1]/2])\n\ndef filterChange():\n    filters = int( tn\['filter'].getValue() )\n    filterQuality = int( tn\['filterQuality'].getValue() )\n    crop = int( tn\['crop'].getValue() )\n    for i in range (0, qualityVal):\n        blurVal = nuke.toNode(fullList\[i*len(nodeList)])\n        blurVal\['filter'].setValue( filters )\n        blurVal\['quality'].setValue( filterQuality )\n        blurVal\['crop'].setValue( crop )\n\n\n##### CREATE THE NODES ###################\n\nif tk.name()=='quality':\n    if qualityVal < 1:\n        qualityVal = 1\n    elif qualityVal >100:\n        qualityVal = 100\n    tn\['quality'].setValue( qualityVal )\n    \n\n    if qualityVal != previousLoopNum:\n\n\n        inputDep = nuke.dependencies(\[nuke.toNode('Unpremult1')], nuke.INPUTS)\n        resultOver = nuke.toNode( inputDep\[0].name() )\n\n        difference = qualityVal-previousLoopNum\n        if difference > 0:\n            for i in range (1, int(difference+1)):\n                \n                blur = nuke.nodes.Blur ()\n                blur\['channels'].setValue('rgba')\n                blur\['maskChannelInput'].setValue('none')\n                blur.setInput (0, nuke.toNode('Switch2'))\n                \n                unpremult = nuke.nodes.Unpremult ()\n                unpremult\['channels'].setValue('rgb')\n                unpremult\['alpha'].setValue('rgba.alpha')\n                unpremult\['invert'].setValue(0)\n                unpremult.setInput (0, blur)\n                \n                expression = nuke.nodes.Expression(expr3 = 'a==0?0:1')\n                expression.setInput (0, unpremult)\n                \n                erode = nuke.nodes.FilterErode ()\n                erode\['channels'].setValue( 'rgba.alpha' )\n                erode\['filter'].setValue( 'gaussian' )\n                erode.setInput (0, expression)\n                \n                premult = nuke.nodes.Premult ()\n                premult\['channels'].setValue( 'rgb' )\n                premult\['alpha'].setValue( 'rgba.alpha' )\n                premult.setInput (0, erode)\n                \n                mergeOver = nuke.nodes.Merge2 ()\n                mergeOver\['operation'].setValue( 'over' )\n                mergeOver\['sRGB'].setValue( 0 )\n                mergeOver\['screen_alpha'].setValue( 0 )\n                mergeOver\['bbox'].setValue( 'union' )\n                mergeOver\['Achannels'].setValue( 'rgba' )\n                mergeOver\['Bchannels'].setValue( 'rgba' )\n                mergeOver\['output'].setValue( 'rgba' )\n                mergeOver\['also_merge'].setValue( 'none' )\n                mergeOver.setInput (0, premult)\n                mergeOver.setInput (1, resultOver)\n                \n                \n                resultOver = mergeOver\n                \n                \n                nodeList = blur.name(), unpremult.name(), expression.name(), erode.name(), premult.name(), resultOver.name()\n                fullList.extend(nodeList)\n\n\n##### DELETE THE NODES ##################################\n\n        else:\n            fullList.reverse()\n            for i in range (int(qualityVal), int(previousLoopNum)):\n                for i in range (0, len(nodeList)):\n                    nuke.delete(nuke.toNode(fullList\[0]))\n                    del fullList\[0]\n                resultOver = nuke.toNode(fullList\[0])\n                resultExpression = nuke.toNode(fullList\[3])\n            fullList.reverse()\n\n        nuke.toNode('Unpremult1').setInput(0, resultOver)\n        valChange()\n        filterChange()\n                \n        previousLoopNum = qualityVal\n        \n        tn\['previousLoopNum'].setValue( int(previousLoopNum) )\n        tn\['fullList'].setValue( str(fullList) )\n\n\n\n##### CHANGE EXISTING VALUES #####################\n\nif tk.name()=='growTexture':\n    valChange()\n    \nif tk.name()=='qualityBias':\n    valChange()\n    \nif tk.name()=='filter':\n    filterChange()\n    \nif tk.name()=='filterQuality':\n    filterChange()\n    \nif tk.name()=='crop':\n    filterChange()\n\n\n"
    tile_color 0xbc9512ff
    selected true
    xpos 4690
    ypos -84
    addUserKnob {20 User l fColourDilate}
    addUserKnob {14 growTexture l "Grow Texture" R 0 100}
    growTexture 15
    addUserKnob {14 erodeMatte l "Erode Matte" t "Dilate or erode the matte to adjust the grow start paint" R -100 100}
    erodeMatte -1.5
    addUserKnob {7 expandMatte l "Expand Matte" t "Expand or compress the matte to adjust the grow start point" R -0.99999 0.99999}
    addUserKnob {7 quality l Iterations t "The number of iterations Colour Dilate will go through.  More is slower.  Max steps is 100" R 1 20}
    quality 6
    addUserKnob {7 qualityBias l Distribution t "0 is a linear distribution.  1 is exponential distribution.\nExponential is smoother with low quality.\nLinear is better with high quality."}
    addUserKnob {4 filter l Filter t "filter\n" M {box triangle quadratic gaussian ""}}
    filter gaussian
    addUserKnob {3 filterQuality l " " -STARTLINE}
    filterQuality 15
    addUserKnob {6 crop l "crop to format" -STARTLINE}
    crop true
    addUserKnob {4 matteOutput l "Matte Output" M {rgb.alpha matte "dilated matte" "dilated area" "" ""}}
    addUserKnob {26 blank l "" -STARTLINE T "      "}
    addUserKnob {6 invertMatte l "invert Matte" -STARTLINE}
    invertMatte true
    addUserKnob {26 blank2 l "" -STARTLINE T "      "}
    addUserKnob {6 rgbIsPremult l "RGB is Premult?" -STARTLINE}
    addUserKnob {1 colorDilate l INVISIBLE t "Keep me hidden" +INVISIBLE}
    addUserKnob {3 previousLoopNum l INVISIBLE +INVISIBLE}
    previousLoopNum 6
    addUserKnob {1 fullList l INVISIBLE +INVISIBLE}
    fullList "\['Blur1', 'Unpremult2', 'Expression1', 'FilterErode2', 'Premult1', 'Merge2', 'Blur2', 'Unpremult3', 'Expression2', 'FilterErode3', 'Premult2', 'Merge4', 'Blur3', 'Unpremult4', 'Expression3', 'FilterErode4', 'Premult3', 'Merge5', 'Blur4', 'Unpremult5', 'Expression4', 'FilterErode5', 'Premult4', 'Merge6', 'Blur5', 'Unpremult6', 'Expression5', 'FilterErode6', 'Premult5', 'Merge7', 'Blur6', 'Unpremult7', 'Expression6', 'FilterErode7', 'Premult6', 'Merge8', 'Blur7', 'Unpremult8', 'Expression7', 'FilterErode8', 'Premult7', 'Merge9']"
    }
    Input {
    inputs 0
    name InputMask
    xpos 803
    ypos -723
    number 1
    }
    Dot {
    name Dot9
    xpos 837
    ypos -657
    }
    set Nb62cdb50 [stack 0]
    AddChannels {
    channels alpha
    name AddChannels2
    xpos 463
    ypos 551
    }
    Dot {
    name Dot3
    xpos 497
    ypos 609
    }
    Input {
    inputs 0
    name InputRGB
    xpos 1019
    ypos -607
    }
    Dot {
    name Dot8
    xpos 1115
    ypos -516
    }
    set Nc354e780 [stack 0]
    AddChannels {
    channels alpha
    name AddChannels1
    xpos 393
    ypos 461
    }
    Dot {
    name Dot2
    xpos 427
    ypos 506
    }
    push $Nb62cdb50
    FilterErode {
    channels alpha
    size {{invertMatte==0?erodeMatte.w:-erodeMatte.w i} {invertMatte==0?erodeMatte.h:-erodeMatte.h i}}
    name FilterErode1
    xpos 803
    ypos -621
    }
    Invert {
    channels alpha
    mix {{invertMatte i}}
    name Invert2
    xpos 803
    ypos -563
    }
    Grade {
    channels alpha
    blackpoint {{"clamp(expandMatte, 0, 1)" i}}
    whitepoint {{"clamp(1+expandMatte, 0, 1)" i}}
    white_clamp true
    name Grade1
    xpos 803
    ypos -505
    }
    Grade {
    channels alpha
    whitepoint 0.5
    black_clamp false
    white_clamp true
    name Grade6
    xpos 803
    ypos -345
    }
    Dot {
    name Dot1
    xpos 837
    ypos -287
    }
    set N71aa5b60 [stack 0]
    push $Nc354e780
    AddChannels {
    channels alpha
    name AddChannels3
    xpos 1150
    ypos -453
    }
    push $Nc354e780
    Shuffle {
    alpha white
    name Shuffle1
    xpos 883
    ypos -469
    }
    Crop {
    box {{InputRGB.bbox.x x1001 542} {InputRGB.bbox.y} {InputRGB.bbox.r} {InputRGB.bbox.t}}
    name Crop3
    selected true
    xpos 1013
    ypos -415
    }
    Switch {
    inputs 2
    which {{rgbIsPremult i}}
    name Switch1
    xpos 1013
    ypos -370
    }
    Dot {
    name Dot5
    xpos 1047
    ypos -326
    }
    set N5c24c1e0 [stack 0]
    Merge2 {
    inputs 2
    operation stencil
    bbox intersection
    name Merge1
    xpos 1013
    ypos -292
    }
    push $N71aa5b60
    Dot {
    name Dot4
    xpos 837
    ypos -253
    }
    push $N5c24c1e0
    Dot {
    name Dot6
    xpos 946
    ypos -326
    }
    Merge2 {
    inputs 2
    operation stencil
    bbox B
    name Merge3
    xpos 912
    ypos -258
    }
    Dot {
    name Dot7
    xpos 946
    ypos -192
    }
    Switch {
    inputs 2
    which {{invertMatte i}}
    name Switch2
    xpos 1013
    ypos -197
    }
    set Na29c9aa0 [stack 0]
    push $Na29c9aa0
    Blur {
    channels rgba
    size 2.5
    name Blur1
    xpos 1336
    ypos -26
    }
    Unpremult {
    name Unpremult2
    xpos 1336
    ypos 18
    }
    Expression {
    expr3 a==0?0:1
    name Expression1
    xpos 1336
    ypos 62
    }
    FilterErode {
    channels alpha
    size 1.25
    filter gaussian
    name FilterErode2
    xpos 1336
    ypos 106
    }
    Premult {
    name Premult1
    xpos 1336
    ypos 164
    }
    Merge2 {
    inputs 2
    name Merge2
    xpos 1336
    ypos 208
    }
    push $Na29c9aa0
    Blur {
    channels rgba
    size 5
    name Blur2
    xpos 1673
    ypos -171
    }
    Unpremult {
    name Unpremult3
    xpos 1673
    ypos -145
    }
    Expression {
    expr3 a==0?0:1
    name Expression2
    xpos 1673
    ypos -119
    }
    FilterErode {
    channels alpha
    size 2.5
    filter gaussian
    name FilterErode3
    xpos 1673
    ypos -93
    }
    Premult {
    name Premult2
    xpos 1673
    ypos -67
    }
    Merge2 {
    inputs 2
    name Merge4
    xpos 1673
    ypos 208
    }
    push $Na29c9aa0
    Blur {
    channels rgba
    size 7.5
    name Blur3
    xpos 1453
    ypos -171
    }
    Unpremult {
    name Unpremult4
    xpos 1453
    ypos -145
    }
    Expression {
    expr3 a==0?0:1
    name Expression3
    xpos 1453
    ypos -119
    }
    FilterErode {
    channels alpha
    size 3.75
    filter gaussian
    name FilterErode4
    xpos 1453
    ypos -93
    }
    Premult {
    name Premult3
    xpos 1453
    ypos 234
    }
    Merge2 {
    inputs 2
    name Merge5
    xpos 1673
    ypos 234
    }
    push $Na29c9aa0
    Blur {
    channels rgba
    size 10
    name Blur4
    xpos 1343
    ypos -171
    }
    Unpremult {
    name Unpremult5
    xpos 1343
    ypos -145
    }
    Expression {
    expr3 a==0?0:1
    name Expression4
    xpos 1343
    ypos -119
    }
    FilterErode {
    channels alpha
    size 5
    filter gaussian
    name FilterErode5
    xpos 1343
    ypos -93
    }
    Premult {
    name Premult4
    xpos 1343
    ypos -67
    }
    Merge2 {
    inputs 2
    name Merge6
    xpos 1673
    ypos 260
    }
    push $Na29c9aa0
    Blur {
    channels rgba
    size 12.5
    name Blur5
    xpos 1233
    ypos -171
    }
    Unpremult {
    name Unpremult6
    xpos 1233
    ypos -145
    }
    Expression {
    expr3 a==0?0:1
    name Expression5
    xpos 1233
    ypos -119
    }
    FilterErode {
    channels alpha
    size 6.25
    filter gaussian
    name FilterErode6
    xpos 1233
    ypos -93
    }
    Premult {
    name Premult5
    xpos 1233
    ypos 286
    }
    Merge2 {
    inputs 2
    name Merge7
    xpos 1673
    ypos 286
    }
    push $Na29c9aa0
    Blur {
    channels rgba
    size 15
    name Blur6
    xpos 1123
    ypos -171
    }
    Unpremult {
    name Unpremult7
    xpos 1123
    ypos -145
    }
    Expression {
    expr3 a==0?0:1
    name Expression6
    xpos 1123
    ypos -119
    }
    FilterErode {
    channels alpha
    size 7.5
    filter gaussian
    name FilterErode7
    xpos 1123
    ypos -93
    }
    Premult {
    name Premult6
    xpos 1123
    ypos 312
    }
    Merge2 {
    inputs 2
    name Merge8
    xpos 1673
    ypos 312
    }
    push $Na29c9aa0
    Blur {
    channels rgba
    size 13.125
    name Blur7
    xpos 1013
    ypos -171
    }
    Unpremult {
    name Unpremult8
    xpos 1013
    ypos -145
    }
    Expression {
    expr3 a==0?0:1
    name Expression7
    xpos 1013
    ypos -119
    }
    FilterErode {
    channels alpha
    size 6.5625
    filter gaussian
    name FilterErode8
    xpos 1013
    ypos -93
    }
    Premult {
    name Premult7
    xpos 1013
    ypos 338
    }
    Merge2 {
    inputs 2
    name Merge9
    xpos 1673
    ypos 338
    }
    Unpremult {
    name Unpremult1
    xpos 269
    ypos 450
    }
    Copy {
    inputs 2
    from0 rgba.alpha
    to0 rgba.alpha
    mix {{matteOutput==0?1:0 i}}
    name Copy2
    xpos 269
    ypos 494
    }
    Copy {
    inputs 2
    from0 rgba.alpha
    to0 rgba.alpha
    mix {{matteOutput==1?1:0}}
    name Copy1
    xpos 269
    ypos 597
    }
    Expression {
    expr3 a<=0.000001?0:1
    mix {{matteOutput==3?1:0}}
    name Expression8
    xpos 269
    ypos 771
    }
    push $Nc354e780
    ShuffleCopy {
    inputs 2
    red red
    green green
    blue blue
    name ShuffleCopy1
    xpos 594
    ypos 771
    }
    Output {
    name Output1
    xpos 594
    ypos 848
    }
    end_group
```
