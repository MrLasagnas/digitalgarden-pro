---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/vector-frame-blend/","tags":["nuke","node","time","frames"]}
---


# VectorFrameBlend

## 🧠 Description


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
    name Vector_FrameBlend1
    knobChanged "\nn = nuke.thisNode()\nk = nuke.thisKnob()\n\nif k.name() == \"useExt\":\n    if k.getValue() == True:\n        n\[\"vecDetail\"].setEnabled(False)\n        n\[\"vecStrength\"].setEnabled(False)\n    else:\n        n\[\"vecDetail\"].setEnabled(True)\n        n\[\"vecStrength\"].setEnabled(True)\n"
    label (all)
    selected true
    xpos -1140
    ypos -5862
    addUserKnob {20 User}
    addUserKnob {41 useGPUIfAvailable l "Use GPU if available" T VectorGenerator1.useGPUIfAvailable}
    addUserKnob {26 ""}
    addUserKnob {4 Frames l "Number of Frames" M {3 5 7 9 11}}
    Frames 5
    addUserKnob {26 ""}
    addUserKnob {7 blurVec l "Vector Blur" R 0 100}
    addUserKnob {7 vecDetail l "Vector Detail"}
    vecDetail 1
    addUserKnob {7 vecStrength l Strength R 0 1.5}
    vecStrength 1.5
    addUserKnob {41 matteChannel l Matte T VectorGenerator1.matteChannel}
    addUserKnob {6 useExt l "use external vectors" +STARTLINE}
    addUserKnob {26 ""}
    addUserKnob {7 mix}
    mix 1
    addUserKnob {26 ""}
    addUserKnob {26 info l "" +STARTLINE T "v1.01 (c) Riley Gray 2022"}
    }
    Input {
    inputs 0
    name Input
    xpos 0
    ypos -23
    }
    Dot {
    name Dot13
    xpos 34
    ypos 138
    }
    set N23788c50 [stack 0]
    Dot {
    name Dot14
    xpos -930
    ypos 138
    }
    set N11f1c710 [stack 0]
    Dot {
    name Dot39
    xpos -1687
    ypos 138
    }
    Dot {
    name Dot38
    xpos -1687
    ypos 1662
    }
    Input {
    inputs 0
    name extVectors
    xpos -1361
    ypos -36
    number 1
    }
    Dot {
    name Dot15
    xpos -1327
    ypos 356
    }
    Input {
    inputs 0
    name Matte
    xpos -1185
    ypos -35
    number 2
    }
    Dot {
    name Dot12
    xpos -1151
    ypos 287
    }
    push $N11f1c710
    Crop {
    box {0 0 {width} {height}}
    name Crop1
    xpos -964
    ypos 224
    }
    VectorGenerator {
    inputs 2
    motionEstimation Regularized
    vectorDetailReg {{parent.vecDetail}}
    strengthReg {{parent.vecStrength}}
    name VectorGenerator1
    xpos -964
    ypos 277
    }
    Switch {
    inputs 2
    which {{parent.useExt}}
    name Switch1
    xpos -964
    ypos 352
    }
    Dot {
    name Dot16
    xpos -930
    ypos 437
    }
    set N75d37ac0 [stack 0]
    Shuffle {
    in backward
    name Shuffle1
    xpos -964
    ypos 503
    }
    Remove {
    operation keep
    channels {rgba.red rgba.green -rgba.blue none}
    name Remove3
    xpos -964
    ypos 529
    }
    Multiply {
    channels rgb
    value {{"proxy? 1/width/proxy_scale:1/width"} {"proxy? 1/height/proxy_scale:1/height"} 1 1}
    name Multiply6
    xpos -964
    ypos 555
    }
    Expression {
    expr0 "(x+0.5)/width + r"
    expr1 "(y+0.5)/height + g"
    name Expression1
    xpos -964
    ypos 581
    }
    Dot {
    name Dot19
    xpos -930
    ypos 678
    }
    set N120019c0 [stack 0]
    Dot {
    name Dot18
    xpos -930
    ypos 862
    }
    set N2c903e60 [stack 0]
    push $N120019c0
    Dot {
    name Dot20
    xpos -1086
    ypos 678
    }
    set N1e733c60 [stack 0]
    TimeOffset {
    time_offset 1
    time ""
    name TimeOffset11
    xpos -1120
    ypos 772
    }
    Dot {
    name Dot24
    xpos -1086
    ypos 959
    }
    STMap {
    inputs 2
    channels rgb
    uv rgb
    name STMap11
    xpos -964
    ypos 955
    }
    set N44c38470 [stack 0]
    push $N1e733c60
    Dot {
    name Dot21
    xpos -1234
    ypos 678
    }
    set N55735bb0 [stack 0]
    TimeOffset {
    time_offset 2
    time ""
    name TimeOffset14
    xpos -1268
    ypos 772
    }
    Dot {
    name Dot25
    xpos -1234
    ypos 1043
    }
    STMap {
    inputs 2
    channels rgb
    uv rgb
    name STMap12
    xpos -964
    ypos 1039
    }
    set Nc887e90 [stack 0]
    push $N55735bb0
    Dot {
    name Dot22
    xpos -1384
    ypos 678
    }
    set N66985f50 [stack 0]
    TimeOffset {
    time_offset 3
    time ""
    name TimeOffset12
    xpos -1418
    ypos 772
    }
    Dot {
    name Dot26
    xpos -1384
    ypos 1138
    }
    STMap {
    inputs 2
    channels rgb
    uv rgb
    name STMap13
    xpos -964
    ypos 1134
    }
    set Nb0a4610 [stack 0]
    push $N66985f50
    Dot {
    name Dot23
    xpos -1528
    ypos 678
    }
    TimeOffset {
    time_offset 4
    time ""
    name TimeOffset13
    xpos -1562
    ypos 772
    }
    Dot {
    name Dot27
    xpos -1528
    ypos 1244
    }
    STMap {
    inputs 2
    channels rgb
    uv rgb
    name STMap14
    xpos -964
    ypos 1240
    }
    Blur {
    channels rgb
    size {{parent.blurVec}}
    name Blur5
    xpos -835
    ypos 1240
    }
    push $N23788c50
    Dot {
    name Dot1
    xpos 34
    ypos 683
    }
    set N70bdcc30 [stack 0]
    Dot {
    name Dot2
    xpos -108
    ypos 683
    }
    set N65e16300 [stack 0]
    Dot {
    name Dot3
    xpos -256
    ypos 683
    }
    set Nf3f251c0 [stack 0]
    Dot {
    name Dot4
    xpos -406
    ypos 683
    }
    set N1e58b4d0 [stack 0]
    Dot {
    name Dot5
    xpos -550
    ypos 683
    }
    set Nf3ffece0 [stack 0]
    Dot {
    name Dot6
    xpos -698
    ypos 683
    }
    TimeOffset {
    time_offset 5
    time ""
    name TimeOffset5
    xpos -732
    ypos 777
    }
    STMap {
    inputs 2
    uv rgb
    name STMap10
    xpos -732
    ypos 1234
    }
    push $Nb0a4610
    Blur {
    channels rgb
    size {{parent.blurVec}}
    name Blur4
    xpos -834
    ypos 1134
    }
    push $Nf3ffece0
    TimeOffset {
    time_offset 4
    time ""
    name TimeOffset4
    xpos -584
    ypos 777
    }
    STMap {
    inputs 2
    uv rgb
    name STMap7
    xpos -584
    ypos 1128
    }
    set N7e3d880 [stack 0]
    push $Nc887e90
    Blur {
    channels rgb
    size {{parent.blurVec}}
    name Blur3
    xpos -834
    ypos 1039
    }
    push $N1e58b4d0
    TimeOffset {
    time_offset 3
    time ""
    name TimeOffset3
    xpos -440
    ypos 777
    }
    STMap {
    inputs 2
    uv rgb
    name STMap5
    xpos -440
    ypos 1033
    }
    set N649f0150 [stack 0]
    push $N44c38470
    Blur {
    channels rgb
    size {{parent.blurVec}}
    name Blur1
    xpos -832
    ypos 955
    }
    push $Nf3f251c0
    TimeOffset {
    time_offset 2
    time ""
    name TimeOffset2
    xpos -290
    ypos 777
    }
    STMap {
    inputs 2
    uv rgb
    name STMap4
    xpos -290
    ypos 949
    }
    set N1e6b0ba0 [stack 0]
    push $N2c903e60
    Blur {
    channels rgb
    size {{parent.blurVec}}
    name Blur2
    xpos -832
    ypos 858
    }
    push $N65e16300
    TimeOffset {
    time_offset 1
    time ""
    name TimeOffset1
    xpos -142
    ypos 777
    }
    STMap {
    inputs 2
    uv rgb
    name STMap1
    xpos -142
    ypos 852
    }
    set N2c95fd70 [stack 0]
    push $N70bdcc30
    push $N75d37ac0
    Dot {
    name Dot17
    xpos 968
    ypos 437
    }
    Shuffle {
    in forward
    name Shuffle2
    xpos 934
    ypos 510
    }
    Remove {
    operation keep
    channels {rgba.red rgba.green -rgba.blue none}
    name Remove1
    xpos 934
    ypos 536
    }
    Multiply {
    channels rgb
    value {{"proxy? 1/width/proxy_scale:1/width"} {"proxy? 1/height/proxy_scale:1/height"} 1 1}
    name Multiply7
    xpos 934
    ypos 562
    }
    Expression {
    expr0 "(x+0.5)/width + r"
    expr1 "(y+0.5)/height + g"
    name Expression2
    xpos 934
    ypos 588
    }
    Dot {
    name Dot28
    xpos 968
    ypos 679
    }
    set Ncbe03a0 [stack 0]
    Dot {
    name Dot33
    xpos 968
    ypos 862
    }
    set N47ffe3e0 [stack 0]
    Blur {
    channels rgb
    size {{parent.blurVec}}
    name Blur6
    xpos 823
    ypos 858
    }
    push $N70bdcc30
    Dot {
    name Dot11
    xpos 173
    ypos 683
    }
    set N780e2d0 [stack 0]
    TimeOffset {
    time_offset -1
    time ""
    name TimeOffset10
    xpos 139
    ypos 777
    }
    STMap {
    inputs 2
    uv rgb
    name STMap2
    xpos 139
    ypos 852
    }
    set N9dd0ff50 [stack 0]
    push $N47ffe3e0
    push $Ncbe03a0
    Dot {
    name Dot29
    xpos 1098
    ypos 679
    }
    set N11eeab60 [stack 0]
    TimeOffset {
    time_offset -1
    time ""
    name TimeOffset17
    xpos 1064
    ypos 773
    }
    Dot {
    name Dot34
    xpos 1098
    ypos 960
    }
    STMap {
    inputs 2
    channels rgb
    uv rgb
    name STMap15
    xpos 934
    ypos 956
    }
    set N113a05f0 [stack 0]
    push $N11eeab60
    Dot {
    name Dot30
    xpos 1246
    ypos 679
    }
    set N55743e50 [stack 0]
    TimeOffset {
    time_offset -2
    time ""
    name TimeOffset18
    xpos 1212
    ypos 773
    }
    Dot {
    name Dot35
    xpos 1246
    ypos 1039
    }
    STMap {
    inputs 2
    channels rgb
    uv rgb
    name STMap16
    xpos 934
    ypos 1035
    }
    set N11f3ea60 [stack 0]
    push $N55743e50
    Dot {
    name Dot31
    xpos 1390
    ypos 679
    }
    set N1dba0a30 [stack 0]
    TimeOffset {
    time_offset -3
    time ""
    name TimeOffset15
    xpos 1356
    ypos 773
    }
    Dot {
    name Dot36
    xpos 1390
    ypos 1134
    }
    STMap {
    inputs 2
    channels rgb
    uv rgb
    name STMap17
    xpos 934
    ypos 1130
    }
    set N1e677de0 [stack 0]
    push $N1dba0a30
    Dot {
    name Dot32
    xpos 1540
    ypos 679
    }
    TimeOffset {
    time_offset -4
    time ""
    name TimeOffset16
    xpos 1506
    ypos 773
    }
    Dot {
    name Dot37
    xpos 1540
    ypos 1244
    }
    STMap {
    inputs 2
    channels rgb
    uv rgb
    name STMap18
    xpos 934
    ypos 1240
    }
    Blur {
    channels rgb
    size {{parent.blurVec}}
    name Blur10
    xpos 831
    ypos 1240
    }
    push $N780e2d0
    Dot {
    name Dot10
    xpos 321
    ypos 683
    }
    set N11f62c50 [stack 0]
    Dot {
    name Dot9
    xpos 465
    ypos 683
    }
    set N113da350 [stack 0]
    Dot {
    name Dot8
    xpos 615
    ypos 683
    }
    set N120c5880 [stack 0]
    Dot {
    name Dot7
    xpos 763
    ypos 683
    }
    TimeOffset {
    time_offset -5
    time ""
    name TimeOffset6
    xpos 729
    ypos 777
    }
    STMap {
    inputs 2
    uv rgb
    name STMap9
    xpos 729
    ypos 1234
    }
    push $N1e677de0
    Blur {
    channels rgb
    size {{parent.blurVec}}
    name Blur9
    xpos 824
    ypos 1130
    }
    push $N120c5880
    TimeOffset {
    time_offset -4
    time ""
    name TimeOffset7
    xpos 581
    ypos 777
    }
    STMap {
    inputs 2
    uv rgb
    name STMap8
    xpos 581
    ypos 1124
    }
    set N7e1f580 [stack 0]
    push 0
    push $N11f3ea60
    Blur {
    channels rgb
    size {{parent.blurVec}}
    name Blur8
    xpos 824
    ypos 1035
    }
    push $N113da350
    TimeOffset {
    time_offset -3
    time ""
    name TimeOffset8
    xpos 431
    ypos 777
    }
    STMap {
    inputs 2
    uv rgb
    name STMap6
    xpos 431
    ypos 1029
    }
    set Nc776930 [stack 0]
    push $N113a05f0
    Blur {
    channels rgb
    size {{parent.blurVec}}
    name Blur7
    xpos 824
    ypos 956
    }
    push $N11f62c50
    TimeOffset {
    time_offset -2
    time ""
    name TimeOffset9
    xpos 287
    ypos 777
    }
    STMap {
    inputs 2
    uv rgb
    name STMap3
    xpos 287
    ypos 950
    }
    set N1201f350 [stack 0]
    Merge2 {
    inputs 11+1
    operation plus
    also_merge all
    name Merge5
    xpos 287
    ypos 1240
    }
    Multiply {
    value 0.09090909091
    name Multiply2
    xpos 287
    ypos 1363
    }
    push $N7e1f580
    push $Nc776930
    push $N1201f350
    push $N9dd0ff50
    push $N70bdcc30
    push $N2c95fd70
    push $N7e3d880
    push 0
    push $N649f0150
    push $N1e6b0ba0
    Merge2 {
    inputs 9+1
    operation plus
    also_merge all
    name Merge4
    xpos -290
    ypos 1134
    }
    Multiply {
    value 0.1111111111
    name Multiply4
    xpos -290
    ypos 1360
    }
    push $Nc776930
    push $N649f0150
    push $N70bdcc30
    push $N1201f350
    push $N2c95fd70
    push 0
    push $N1e6b0ba0
    push $N9dd0ff50
    Merge2 {
    inputs 7+1
    operation plus
    also_merge all
    name Merge3
    xpos 139
    ypos 1035
    }
    Multiply {
    value 0.1428571429
    name Multiply1
    xpos 139
    ypos 1362
    }
    push $N70bdcc30
    push $N1201f350
    push $N9dd0ff50
    push 0
    push $N1e6b0ba0
    push $N2c95fd70
    Merge2 {
    inputs 5+1
    operation plus
    also_merge all
    name Merge2
    xpos -142
    ypos 955
    }
    Multiply {
    value 0.2
    name Multiply3
    xpos -142
    ypos 1359
    }
    push $N9dd0ff50
    push 0
    push $N2c95fd70
    push $N70bdcc30
    Merge2 {
    inputs 3+1
    operation plus
    also_merge all
    name Merge1
    xpos 0
    ypos 858
    }
    Multiply {
    value 0.3333333333
    name Multiply5
    xpos 0
    ypos 1357
    }
    Switch {
    inputs 5
    which {{parent.Frames}}
    name Switch2
    xpos 0
    ypos 1507
    }
    Dissolve {
    inputs 2
    which {{1-parent.mix}}
    name Dissolve1
    xpos 0
    ypos 1652
    }
    Output {
    name Output1
    xpos 0
    ypos 1812
    }
    end_group
```
