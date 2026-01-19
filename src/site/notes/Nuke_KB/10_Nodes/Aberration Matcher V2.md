---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/aberration-matcher-v2/","tags":["nuke","node","aberration","lens"]}
---


# Aberration Matcher V2

## 🧠 Description
Setup to match plate aberration
## 🎯 When to use


## ⚠️ Known pitfalls


## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code
```nuke
    set cut_paste_input [stack 0]
    version 13.2 v8
    push 0
    push $cut_paste_input
    Group {
    inputs 2
    name ABERRATION_MATCHER
    tile_color 0x65097cff
    selected true
    xpos -7177
    ypos -1598
    addUserKnob {20 User}
    addUserKnob {6 remove_aberration l "Remove Aberration" +STARTLINE}
    addUserKnob {6 enable_noTimeBlur l "Enable NoTimeBlur" t "Enable a NoTimeBlur node if your CG input has some retime nodes. Can create unwanted results if not." +STARTLINE}
    addUserKnob {6 use_mask l "Use Mask" +STARTLINE}
    addUserKnob {26 coucou l "By G-Raw" T ""}
    }
    Input {
    inputs 0
    name MASK
    xpos -354
    ypos -789
    number 2
    }
    Dot {
    name Dot253
    label MASK
    note_font "Verdana Bold"
    note_font_size 20
    xpos -323
    ypos -692
    }
    Dot {
    name Dot215
    xpos -323
    ypos -523
    }
    Input {
    inputs 0
    name PLATE
    xpos -506
    ypos -1342
    number 1
    }
    Dot {
    name Dot216
    label PLATE
    note_font "Bitstream Vera Sans Bold"
    note_font_size 30
    xpos -475
    ypos -1210
    }
    Dot {
    name Dot255
    xpos -475
    ypos -1062
    }
    set N21b8fcf0 [stack 0]
    TimeWarp {
    lookup {{t>>2 C x995 995 x1092 1092}}
    time ""
    filter none
    name TimeWarp17
    xpos -506
    ypos -947
    }
    Dot {
    name Dot217
    xpos -475
    ypos -891
    }
    set N1fe91b90 [stack 0]
    Dot {
    name Dot218
    xpos -619
    ypos -891
    }
    set N12fabb10 [stack 0]
    Dot {
    name Dot219
    xpos -764
    ypos -891
    }
    Shuffle1 {
    green red
    blue red
    alpha red
    name Shuffle33
    label RED
    xpos -795
    ypos -802
    }
    Dot {
    name Dot220
    xpos -764
    ypos -662
    }
    push $N12fabb10
    Shuffle1 {
    red green
    blue green
    alpha green
    name Shuffle35
    label GREEN
    xpos -650
    ypos -802
    }
    Dot {
    name Dot243
    xpos -619
    ypos -721
    }
    push $N1fe91b90
    Shuffle1 {
    red blue
    green blue
    alpha blue
    name Shuffle36
    label BLUE
    xpos -509
    ypos -819
    }
    Switch {
    inputs 2
    which {{t%2}}
    name Switch10
    xpos -509
    ypos -722
    }
    Switch {
    inputs 2
    which {{!(t%4)}}
    name Switch11
    xpos -509
    ypos -663
    }
    VectorGenerator {
    motionEstimation Regularized
    vectorDetailReg 1
    strengthReg 1
    Advanced 1
    flickerCompensation true
    name VectorGenerator5
    xpos -509
    ypos -587
    }
    Multiply {
    inputs 1+1
    channels motion
    value 0.04
    name Multiply8
    xpos -509
    ypos -530
    disable {{"parent.use_mask == 0" K x3972 1}}
    }
    Blur {
    channels motion
    size 100
    name Blur16
    label "\[value size]"
    xpos -509
    ypos -472
    }
    Dot {
    name Dot244
    xpos -478
    ypos -395
    }
    push $N21b8fcf0
    Input {
    inputs 0
    name CG
    xpos 400
    ypos -1286
    }
    Dot {
    name Dot245
    label CG
    note_font "Bitstream Vera Sans Bold"
    note_font_size 30
    xpos 431
    ypos -1210
    }
    Switch {
    inputs 2
    which {{parent.remove_aberration}}
    name Switch12
    xpos 400
    ypos -1063
    }
    Dot {
    name Dot254
    xpos 431
    ypos -618
    }
    set N234809c0 [stack 0]
    Dot {
    name Dot246
    xpos -162
    ypos -618
    }
    NoTimeBlur {
    name NoTimeBlur5
    xpos -193
    ypos -544
    disable {{"parent.enable_noTimeBlur == 0"}}
    }
    TimeWarp {
    lookup {{t>>2 C x995 995 x1092 1092}}
    time ""
    filter none
    name TimeWarp18
    xpos -193
    ypos -492
    }
    Dot {
    name Dot247
    xpos -162
    ypos -457
    }
    Dot {
    name Dot248
    xpos -303
    ypos -457
    }
    ShuffleCopy {
    inputs 2
    in motion
    red red
    green green
    blue blue
    out motion
    name ShuffleCopy4
    xpos -334
    ypos -396
    }
    IDistort {
    channels rgba
    uv motion
    uv_scale {{"parent.remove_aberration * -2 + 1"}}
    name IDistort4
    xpos -337
    ypos -320
    }
    set N31390b80 [stack 0]
    Dot {
    name Dot249
    xpos -431
    ypos -319
    }
    TimeOffset {
    time_offset -2
    time ""
    name TimeOffset5
    label "\[value time_offset]"
    xpos -462
    ypos -276
    }
    TimeWarp {
    lookup {{t<<2 C x995 995 x1092 1092}}
    time ""
    filter none
    name TimeWarp19
    xpos -462
    ypos -223
    }
    Dot {
    name Dot250
    xpos -431
    ypos -121
    }
    push $N31390b80
    TimeWarp {
    lookup {{t<<2 C x995 995 x1092 1092}}
    time ""
    filter none
    name TimeWarp20
    xpos -337
    ypos -270
    }
    Dot {
    name Dot251
    xpos -306
    ypos -218
    }
    push $N234809c0
    Copy {
    inputs 2
    from0 rgba.red
    to0 rgba.red
    bbox B
    name Copy14
    xpos 400
    ypos -225
    }
    Copy {
    inputs 2
    from0 rgba.blue
    to0 rgba.blue
    bbox B
    name Copy15
    xpos 400
    ypos -128
    }
    Dot {
    name Dot252
    label OUT
    note_font "Verdana Bold"
    note_font_size 50
    xpos 431
    ypos 48
    }
    Output {
    name Output1
    xpos 400
    ypos 138
    }
    StickyNote {
    inputs 0
    name StickyNote11
    tile_color 0xa39934ff
    label "Vectors adjustments\n"
    note_font Verdana
    note_font_size 15
    xpos -704
    ypos -539
    }
    Grade {
    inputs 0
    black_clamp false
    name Grade1
    selected true
    xpos -31
    ypos -840
    }
    end_group
```
