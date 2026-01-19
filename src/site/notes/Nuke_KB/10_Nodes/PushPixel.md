---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/push-pixel/","tags":["nuke","node","edges"]}
---


# PushPixel

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
    push $cut_paste_input
    Group {
    inputs 2
    name Push1
    help "Push or Pull RGB pixels using the Alpha channel as mask.\n\nFor a best result you can combine several PushPixels.\n\n(beta version)"
    tile_color 0xbc9512ff
    label "-\nIntermediate"
    note_font "Verdana Bold"
    note_font_size 10
    selected true
    xpos 4580
    ypos -20
    mapsize {0.15 0.15}
    addUserKnob {20 menu l Push-Pull}
    addUserKnob {4 push_pull l mode M {Push Pull}}
    addUserKnob {41 uv_scale l size T IDistort1.uv_scale}
    addUserKnob {16 edge_size l Edge_size R 0 3}
    edge_size {{IDistort1.uv_scale}}
    addUserKnob {41 size l "Soft select" T Blur1.size}
    addUserKnob {26 ""}
    addUserKnob {6 reformater l "Reformat (root.format)" +STARTLINE}
    addUserKnob {4 alpha_output l "Output alpha" M {"keep RGB input alpha" "keep Mask input alpha" "keep RGB (no alpha)" "keep RGB pushed alpha" "" "" "" "" "" ""}}
    alpha_output "keep RGB (no alpha)"
    }
    Constant {
    inputs 0
    channels rgb
    color 0.5
    name Constant1
    label "Root Format:\n\[value format]"
    xpos -737
    ypos 471
    }
    set N704d0690 [stack 0]
    Dot {
    name Dot9
    xpos -529
    ypos 510
    }
    Transform {
    translate {{parent.edge_size i} {parent.edge_size i}}
    center {960 540}
    name Trans_value1
    xpos -563
    ypos 546
    }
    Input {
    inputs 0
    name InputMask
    xpos -461
    ypos 262
    number 1
    }
    Dot {
    name Dot7
    xpos -427
    ypos 325
    }
    set N39402c70 [stack 0]
    Shuffle {
    red alpha
    green alpha
    blue alpha
    name a_to_rgb
    xpos -461
    ypos 371
    }
    set Ne82fcb20 [stack 0]
    push $Ne82fcb20
    Dot {
    name Dot3
    xpos -427
    ypos 435
    }
    set N2b747500 [stack 0]
    Dot {
    name Dot1
    xpos -427
    ypos 624
    }
    set Neb223a30 [stack 0]
    ColorCorrect {
    gain 0.5
    name ColorCorrect1
    xpos -323
    ypos 921
    }
    push $Neb223a30
    Transform {
    translate {{(parent.Trans_value1.translate.x)*(-1) i} 0}
    center {960 540}
    name Transform1
    xpos -329
    ypos 816
    }
    ColorCorrect {
    gain 0
    name ColorCorrect6
    xpos -329
    ypos 843
    }
    push $Neb223a30
    Transform {
    translate {{parent.Trans_value1.translate.x i} 0}
    center {960 540}
    name Transform6
    xpos -331
    ypos 761
    }
    push $N704d0690
    Dot {
    name Dot2
    xpos -703
    ypos 643
    }
    set N71c1e320 [stack 0]
    Merge2 {
    inputs 2
    name Merge1
    xpos -433
    ypos 761
    }
    Merge2 {
    inputs 2
    name Merge8
    xpos -433
    ypos 843
    }
    Merge2 {
    inputs 2
    name Merge9
    xpos -433
    ypos 921
    }
    push $Neb223a30
    ColorCorrect {
    gain 0.5
    name ColorCorrect7
    xpos -607
    ypos 919
    }
    push $Neb223a30
    Transform {
    translate {0 {parent.Trans_value1.translate.y*-1 i}}
    center {960 540}
    name Transform7
    xpos -609
    ypos 813
    }
    ColorCorrect {
    gain 0
    name ColorCorrect8
    xpos -609
    ypos 837
    }
    push $Neb223a30
    Transform {
    translate {0 {parent.Trans_value1.translate.y i}}
    center {960 540}
    name Transform8
    xpos -608
    ypos 758
    }
    push $N71c1e320
    Merge2 {
    inputs 2
    name Merge10
    xpos -737
    ypos 758
    }
    Merge2 {
    inputs 2
    name Merge11
    xpos -737
    ypos 837
    }
    Merge2 {
    inputs 2
    name Merge12
    xpos -737
    ypos 919
    }
    ShuffleCopy {
    inputs 2
    red red
    blue black
    alpha white
    name ShuffleCopy1
    xpos -551
    ypos 1006
    }
    Blur {
    size {{parent.IDistort1.uv_scale}}
    name Blur1
    xpos -551
    ypos 1051
    }
    set Nd3622090 [stack 0]
    push $Nd3622090
    Invert {
    name Invert2
    xpos -441
    ypos 1079
    }
    Switch {
    inputs 2
    which {{push_pull i}}
    name Switch2
    label "0=Push\n1=Pull"
    xpos -551
    ypos 1113
    }
    push $N2b747500
    Input {
    inputs 0
    name InputRGB
    xpos -866
    ypos 263
    }
    Dot {
    name Dot5
    xpos -832
    ypos 330
    }
    set Nb5b99540 [stack 0]
    Copy {
    inputs 2
    from0 rgba.alpha
    to0 rgba.alpha
    name Copy2
    xpos -866
    ypos 425
    }
    add_layer {push push.x push.y}
    Copy {
    inputs 2
    from0 rgba.red
    to0 push.x
    from1 rgba.green
    to1 push.y
    name Copy1
    xpos -867
    ypos 1113
    }
    set N71c223e0 [stack 0]
    Viewer {
    inputs 2
    input_process false
    name Viewer1
    xpos -815
    ypos 1778
    }
    push $N71c223e0
    IDistort {
    channels rgba
    uv push
    uv_offset 0.5
    uv_scale 6.5
    blur_scale 0
    name IDistort1
    xpos -867
    ypos 1181
    }
    Dot {
    name Dot11
    xpos -833
    ypos 1220
    }
    set N32735e70 [stack 0]
    Dot {
    name Dot14
    xpos -723
    ypos 1220
    }
    set N65ec980 [stack 0]
    Dot {
    name Dot17
    xpos -613
    ypos 1220
    }
    Remove {
    operation keep
    channels rgba
    name Remove3
    xpos -647
    ypos 1330
    }
    Dot {
    name Dot16
    xpos -613
    ypos 1446
    }
    push $N65ec980
    Remove {
    operation keep
    channels rgb
    name Remove2
    xpos -757
    ypos 1326
    }
    Dot {
    name Dot15
    xpos -723
    ypos 1415
    }
    push $N39402c70
    Dot {
    name Dot4
    xpos -200
    ypos 325
    }
    Dot {
    name Dot6
    xpos -200
    ypos 1294
    }
    push $N32735e70
    Copy {
    inputs 2
    from0 rgba.alpha
    to0 rgba.alpha
    name Copy3
    xpos -867
    ypos 1284
    }
    Remove {
    operation keep
    channels rgba
    name Remove4
    xpos -867
    ypos 1346
    }
    push $Nb5b99540
    Dot {
    name Dot8
    xpos -1145
    ypos 330
    }
    Dot {
    name Dot10
    xpos -1145
    ypos 1295
    }
    push $N32735e70
    Dot {
    name Dot12
    xpos -943
    ypos 1220
    }
    Copy {
    inputs 2
    from0 rgba.alpha
    to0 rgba.alpha
    name Copy4
    xpos -977
    ypos 1285
    }
    Remove {
    operation keep
    channels rgba
    name Remove1
    xpos -977
    ypos 1341
    }
    Dot {
    name Dot13
    xpos -943
    ypos 1451
    }
    Switch {
    inputs 4
    which {{alpha_output}}
    name Switch1
    xpos -867
    ypos 1447
    }
    Reformat {
    name Reformat1
    xpos -867
    ypos 1527
    disable {{!reformater}}
    }
    Output {
    name Output1
    xpos -867
    ypos 1660
    }
    end_group
```
