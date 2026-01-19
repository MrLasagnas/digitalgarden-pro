---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/rs-guided-blur-fixed/","tags":["nuke","node","edges","color"]}
---


# RS Guided Blur fixed

## 🧠 Description


## 🎯 When to use


## ⚠️ Known pitfalls


## 🧩 Dependencies

## 🧪 Alternatives

- [[Nuke_KB/10_Nodes/Rs Guided Blur\|Rs Guided Blur]]

## 🧾 Code
```nuke
    set cut_paste_input [stack 0]
    version 13.2 v8
    push $cut_paste_input
    push $cut_paste_input
    Group {
    inputs 2
    name RS_GuidedBlur4
    help "Description:\n\nThe guided filter is an edge-preserving blur,\nuseful to add details to the edges of a roto/matte\nusing a second image as a guide. \n\nHow to use:\n\nConnect the alpha to the 'matte' input\nand the 'guide' image to the 'img' input.\n\nControl the amount of detail using\nthe 'edge detail' slider.\n\n---\n\nThe filter is an implementation of the\n 'Guided Image Filtering', by Kaiming He. \nMore details at http://kaiminghe.com/eccv10/\n"
    label fixed
    selected true
    xpos -2960
    ypos -1917
    addUserKnob {20 GuidedBlur}
    addUserKnob {41 blur_size l "blur size" T Blur1.size}
    addUserKnob {41 tolerance l "edge detail" t "Control the amount of detail that will be preserved. \n\nA value close to 0 makes the filter behave like a regular blur. \n\nValues close to 6 or 7 can introduce artifacts, identifying grain \"as edges\". \n\nRecommended values are between 2.5 and 4." T Constant_eps.multiplier}
    addUserKnob {41 filter T Blur1.filter}
    addUserKnob {41 quality l "" -STARTLINE T Blur1.quality}
    addUserKnob {20 info l Info}
    addUserKnob {26 versionInfo l "" +STARTLINE T "\nCreated by Rafael Silva\nwww.rafael.ai\nrafael@rafael.ai\n\nVersion 1.3\nLast Updated: August 16th, 2020\n\nToronto, Canada\n\n---\n\nHelp and more information:\nwww.rafael.ai/guided_blur"}
    addUserKnob {26 ""}
    }
    Input {
    inputs 0
    name img
    label i
    xpos -40
    ypos -15
    number 1
    }
    Grade {
    white 0.02
    name Grade1
    xpos -40
    ypos 107
    }
    Dot {
    name Dot16
    xpos -6
    ypos 330
    }
    set Na5f16ed0 [stack 0]
    Dot {
    name Dot4
    xpos 544
    ypos 498
    }
    set N6c6ecf10 [stack 0]
    Dot {
    name Dot6
    xpos 654
    ypos 498
    }
    set N8cbc06b0 [stack 0]
    Dot {
    name Dot7
    xpos 874
    ypos 498
    }
    set N7b8bdc90 [stack 0]
    Dot {
    name Dot8
    xpos 984
    ypos 498
    }
    set N6827b1f0 [stack 0]
    Dot {
    name Dot15
    xpos 2194
    ypos 498
    }
    set N71b4f600 [stack 0]
    Dot {
    name Dot3
    xpos 2744
    ypos 498
    }
    NoOp {
    name I1
    xpos 2710
    ypos 1695
    }
    Input {
    inputs 0
    name matte
    label p
    xpos 2160
    ypos 95
    }
    Shuffle {
    red alpha
    green alpha
    blue alpha
    name Shuffle5
    xpos 2050
    ypos 543
    }
    set N959a2800 [stack 0]
    push $N71b4f600
    Merge {
    inputs 2
    operation mult
    name Merge5
    xpos 2160
    ypos 591
    }
    Blur {
    size 9.4
    name Blur1
    xpos 2160
    ypos 635
    }
    set C983a8c70 [stack 0]
    NoOp {
    name mean_Ip
    xpos 2160
    ypos 711
    }
    push $N6c6ecf10
    clone $C983a8c70 {
    xpos 510
    ypos 659
    selected false
    }
    NoOp {
    name mean_I
    xpos 510
    ypos 735
    }
    Dot {
    name Dot9
    xpos 544
    ypos 786
    }
    set N92575ab0 [stack 0]
    Dot {
    name Dot13
    xpos 874
    ypos 810
    }
    set Na2cf8090 [stack 0]
    Dot {
    name Dot14
    xpos 1424
    ypos 834
    }
    set N7b8c13c0 [stack 0]
    push $N959a2800
    clone $C983a8c70 {
    xpos 2050
    ypos 635
    selected false
    }
    NoOp {
    name mean_p
    xpos 2050
    ypos 687
    }
    set N9dcf5d20 [stack 0]
    Merge {
    inputs 2
    operation mult
    name Merge8
    xpos 1940
    ypos 951
    }
    Merge {
    inputs 2
    operation minus
    name Merge9
    xpos 1940
    ypos 1023
    }
    NoOp {
    name cov_Ip
    xpos 1940
    ypos 1095
    }
    set N2f789fc0 [stack 0]
    Constant {
    inputs 0
    channels rgb
    color {{"1/pow(10, multiplier)"}}
    name Constant_eps
    xpos 400
    ypos 1067
    addUserKnob {20 User}
    addUserKnob {7 multiplier R 1 6}
    multiplier 10
    }
    set N84efe8b0 [stack 0]
    push $N8cbc06b0
    push $N7b8bdc90
    Merge2 {
    inputs 2
    operation multiply
    name Merge18
    label I*I
    xpos 730
    ypos 611
    }
    clone $C983a8c70 {
    xpos 730
    ypos 683
    selected false
    }
    set Nce748110 [stack 0]
    Dot {
    name Dot5
    xpos 2634
    ypos 834
    }
    NoOp {
    name mean_II
    xpos 2600
    ypos 1023
    }
    push $N7b8c13c0
    Dot {
    name Dot1
    xpos 2524
    ypos 882
    }
    set N6850ccb0 [stack 0]
    push $N6850ccb0
    Merge2 {
    inputs 2
    operation multiply
    name Merge4
    label "mean I * mean I"
    xpos 2490
    ypos 1019
    }
    Merge2 {
    inputs 2
    operation minus
    name Merge2
    xpos 2490
    ypos 1095
    }
    NoOp {
    name var_I
    xpos 2490
    ypos 1167
    }
    Merge {
    inputs 2
    operation plus
    bbox B
    name Merge13
    xpos 2490
    ypos 1287
    }
    Merge {
    inputs 2
    operation divide
    bbox B
    name Merge14
    xpos 2490
    ypos 1407
    }
    NoOp {
    name a_mono
    label a
    xpos 2490
    ypos 1475
    }
    set N5c7bfcd0 [stack 0]
    clone $C983a8c70 {
    xpos 2600
    ypos 1691
    selected false
    }
    Merge {
    inputs 2
    operation mult
    bbox B
    name Merge17
    xpos 2600
    ypos 1815
    }
    push $N9dcf5d20
    Dot {
    name Dot19
    xpos 2084
    ypos 1410
    }
    set N60ca6030 [stack 0]
    push $N7b8c13c0
    push $N5c7bfcd0
    Merge {
    inputs 2
    operation mult
    bbox B
    name Merge15
    xpos 2490
    ypos 1551
    }
    Merge {
    inputs 2
    operation minus
    bbox B
    name Merge7
    xpos 2490
    ypos 1671
    }
    NoOp {
    name b_mono
    xpos 2490
    ypos 1743
    }
    clone $C983a8c70 {
    xpos 2490
    ypos 1811
    selected false
    }
    NoOp {
    name mean_b_mono_
    xpos 2490
    ypos 1887
    }
    Merge {
    inputs 2
    operation plus
    bbox B
    name Merge16
    xpos 2490
    ypos 1959
    }
    Dot {
    name Dot18
    xpos 2524
    ypos 2922
    }
    push $N60ca6030
    Dot {
    name Dot10
    xpos 1314
    ypos 2346
    }
    Dot {
    name Dot2
    xpos 1314
    ypos 2562
    }
    push $N84efe8b0
    push $Nce748110
    push $N92575ab0
    Expression {
    expr0 r*r
    expr1 g*g
    expr2 b*b
    name Expression3
    label "I*I (rr, gg, bb)"
    xpos 510
    ypos 995
    }
    Merge {
    inputs 2
    operation minus
    name Merge11
    xpos 730
    ypos 1119
    }
    Merge {
    inputs 2
    operation plus
    bbox B
    name Merge12
    xpos 730
    ypos 1215
    }
    NoOp {
    name I___
    label "rr, gg, bb"
    xpos 730
    ypos 1283
    }
    set N51cf8010 [stack 0]
    push $N6827b1f0
    Expression {
    expr0 r*g
    expr1 r*b
    expr2 g*b
    name Expression2
    label "rg, rb, gb"
    xpos 950
    ypos 611
    }
    clone $C983a8c70 {
    xpos 950
    ypos 683
    selected false
    }
    push $Na2cf8090
    Expression {
    expr0 r*g
    expr1 r*b
    expr2 g*b
    name Expression4
    label "rg, rb, gb"
    xpos 840
    ypos 1019
    }
    Merge {
    inputs 2
    operation minus
    name Merge10
    xpos 950
    ypos 1095
    }
    NoOp {
    name I____
    label "rg, rb, gb"
    xpos 950
    ypos 1283
    }
    set N60396a70 [stack 0]
    MergeExpression {
    inputs 2
    expr0 Ar*Ab-Bg*Bg
    expr1 Bg*Br-Ar*Bb
    expr2 Ar*Ag-Br*Br
    name MergeExpression2
    label "invgg, invgb, invbb"
    xpos 730
    ypos 1691
    }
    push $N51cf8010
    push $N60396a70
    MergeExpression {
    inputs 2
    expr0 Ag*Ab-Bb*Bb
    expr1 Bb*Bg-Br*Ab
    expr2 Br*Bb-Ag*Bg
    name MergeExpression1
    label "invrr, invrg, invrb"
    xpos 510
    ypos 1403
    }
    set N6aee80e0 [stack 0]
    push $N60396a70
    MergeExpression {
    inputs 2
    expr0 Ab*Bg
    expr1 0
    expr2 0
    name MergeExpression5
    label invrb*I_rb
    xpos 1170
    ypos 1475
    }
    Shuffle {
    green red
    blue red
    alpha red
    name Shuffle8
    xpos 1170
    ypos 1551
    }
    push 0
    push $N6aee80e0
    push $N51cf8010
    MergeExpression {
    inputs 2
    expr0 Ar*Br
    expr1 0
    expr2 0
    name MergeExpression3
    label invrr*I_rr
    xpos 620
    ypos 1475
    }
    Shuffle {
    green red
    blue red
    alpha red
    name Shuffle2
    xpos 620
    ypos 1527
    }
    push $N6aee80e0
    push $N60396a70
    MergeExpression {
    inputs 2
    expr0 Ag*Br
    expr1 0
    expr2 0
    name MergeExpression4
    label invrg*I_rg
    xpos 950
    ypos 1499
    }
    Shuffle {
    green red
    blue red
    alpha red
    name Shuffle3
    xpos 950
    ypos 1551
    }
    Merge2 {
    inputs 3+1
    operation plus
    name Merge1
    label covDet
    xpos 950
    ypos 1691
    }
    set Naa393320 [stack 0]
    MergeExpression {
    inputs 2
    expr0 Ar/Br
    expr1 Ag/Bg
    expr2 Ab/Bb
    expr3 0
    name MergeExpression7
    label "invgg, invgb, invbb"
    xpos 730
    ypos 1883
    }
    set N972fcdb0 [stack 0]
    push $N6aee80e0
    push $Naa393320
    MergeExpression {
    inputs 2
    expr0 Ar/Br
    expr1 Ag/Bg
    expr2 Ab/Bb
    expr3 0
    name MergeExpression6
    label "invrr, invrg, invrb"
    xpos 510
    ypos 1883
    }
    set Nbb48a6e0 [stack 0]
    ShuffleCopy {
    inputs 2
    red blue2
    green green
    blue blue
    alpha black
    name ShuffleCopy1
    label "invrb, invgb, invbb"
    xpos 840
    ypos 2099
    }
    push $N2f789fc0
    NoOp {
    name cov_Ip1
    xpos 950
    ypos 1887
    }
    Dot {
    name Dot17
    xpos 984
    ypos 1986
    }
    set Naa0bc440 [stack 0]
    MergeExpression {
    inputs 2
    expr0 Ar*Bb
    expr1 Ag*Bb
    expr2 Ab*Bb
    name MergeExpression10
    xpos 840
    ypos 2175
    }
    push 0
    push $Nbb48a6e0
    push $Naa0bc440
    MergeExpression {
    inputs 2
    expr0 Ar*Br
    expr1 Ag*Br
    expr2 Ab*Br
    name MergeExpression8
    xpos 620
    ypos 2175
    }
    push $N972fcdb0
    push $Nbb48a6e0
    ShuffleCopy {
    inputs 2
    red green2
    green red
    blue green
    alpha black
    name ShuffleCopy2
    label "invrg, invgg, invgb"
    xpos 730
    ypos 2099
    }
    push $Naa0bc440
    MergeExpression {
    inputs 2
    expr0 Ar*Bg
    expr1 Ag*Bg
    expr2 Ab*Bg
    name MergeExpression9
    xpos 730
    ypos 2175
    }
    Merge2 {
    inputs 3+1
    operation plus
    name A
    label "a_r, a_g, a_b\n-"
    xpos 730
    ypos 2288
    }
    set Neecf1770 [stack 0]
    push $N7b8c13c0
    NoOp {
    name NoOp2
    label mean_I
    xpos 1390
    ypos 2374
    }
    Merge2 {
    inputs 2
    operation multiply
    name Merge22
    xpos 1390
    ypos 2487
    }
    set N729f9960 [stack 0]
    Shuffle {
    green red
    blue red
    alpha red
    name a_r_mult_mean_I_r
    xpos 1390
    ypos 2559
    }
    Merge2 {
    inputs 2
    operation minus
    name Merge6
    xpos 1390
    ypos 2679
    }
    push $N729f9960
    Shuffle {
    red green
    blue green
    alpha green
    name a_g_mult_mean_I_g
    xpos 1500
    ypos 2559
    }
    Merge2 {
    inputs 2
    operation minus
    name Merge19
    xpos 1500
    ypos 2727
    }
    push $N729f9960
    Shuffle {
    red blue
    green blue
    alpha blue
    name a_b_mult_mean_I_b
    xpos 1610
    ypos 2559
    }
    Merge2 {
    inputs 2
    operation minus
    name Merge20
    xpos 1610
    ypos 2775
    }
    NoOp {
    name NoOp4
    label B
    xpos 1610
    ypos 2819
    }
    Expression {
    channel0 {rgba.red -rgba.green -rgba.blue none}
    expr0 "isnan(r)? r(x+1,y):r"
    channel1 {-rgba.red rgba.green -rgba.blue none}
    expr1 "isnan(r)? r(x+1,y):g"
    channel2 {-rgba.red -rgba.green rgba.blue none}
    expr2 "isnan(r)? r(x+1,y):b"
    expr3 "isnan(r)? r(x+1,y):a"
    name killNan1
    xpos 1610
    ypos 2876
    addUserKnob {20 User}
    addUserKnob {6 xcloneoffset_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
    }
    clone $C983a8c70 {
    xpos 1610
    ypos 2918
    selected false
    }
    NoOp {
    name NoOp5
    label boxfilter(b)
    xpos 1610
    ypos 2987
    }
    push $Neecf1770
    Expression {
    channel0 {rgba.red -rgba.green -rgba.blue none}
    expr0 "isnan(r)? r(x+1,y):r"
    channel1 {-rgba.red rgba.green -rgba.blue none}
    expr1 "isnan(r)? r(x+1,y):g"
    channel2 {-rgba.red -rgba.green rgba.blue none}
    expr2 "isnan(r)? r(x+1,y):b"
    expr3 "isnan(r)? r(x+1,y):a"
    name killNan
    xpos 730
    ypos 2372
    addUserKnob {20 User}
    addUserKnob {6 xcloneoffset_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
    }
    clone $C983a8c70 {
    xpos 730
    ypos 2427
    selected false
    }
    push $Na5f16ed0
    Merge2 {
    inputs 2
    operation multiply
    name Merge3
    xpos -40
    ypos 2799
    }
    NoOp {
    name NoOp6
    label "boxfilter(a_\[rgb])"
    xpos -40
    ypos 3125
    }
    set N3739ee60 [stack 0]
    Shuffle {
    red blue
    green blue
    alpha blue
    name ab2
    xpos 180
    ypos 3327
    }
    push 0
    push $N3739ee60
    Shuffle {
    green red
    blue red
    alpha red
    name ar2
    label "boxfiler(a_r,r) * I_r"
    xpos -40
    ypos 3323
    }
    push $N3739ee60
    Shuffle {
    red green
    blue green
    alpha green
    name ag2
    xpos 70
    ypos 3326
    }
    Merge2 {
    inputs 4+1
    operation plus
    name Merge25
    xpos -40
    ypos 3447
    }
    set N4645f730 [stack 0]
    Dot {
    name Dot11
    xpos -6
    ypos 3546
    }
    Switch {
    inputs 2
    name Switch1
    xpos -40
    ypos 3615
    }
    push $Na5f16ed0
    Dot {
    name Dot12
    xpos -172
    ypos 450
    }
    Dot {
    name Dot20
    xpos -189
    ypos 3610
    }
    Copy {
    inputs 2
    from0 rgba.alpha
    to0 rgba.alpha
    name Copy1
    xpos -40
    ypos 3690
    }
    Output {
    name Output1
    xpos -40
    ypos 3794
    }
    push $N4645f730
    Viewer {
    frame_range 1001-1335
    gain 0.14
    name Viewer1
    xpos -370
    ypos 3481
    }
    end_group
```
