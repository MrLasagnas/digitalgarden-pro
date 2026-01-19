---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/despill-bg/","tags":["nuke","node","despill","keying"]}
---


# DespillBG

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
    name Despill_BG
    help "Remove red/green/blue spill."
    tile_color 0x733520ff
    selected true
    xpos -17529
    ypos 8927
    addUserKnob {20 DESPILL_MAIN}
    addUserKnob {26 ""}
    addUserKnob {26 BackColorSelector l "" +STARTLINE T ##BackColorSelector}
    addUserKnob {4 back_color l ScreenColor t "Which type of spill to remove." M {red green blue "" "" "" "" "" ""}}
    back_color green
    addUserKnob {26 DividerPart1 l "" +STARTLINE T ##Pick_WhitePoint_ScreenColor}
    addUserKnob {6 Show_WhitePoint l "Where pick WhitePoint?? --HERE!" t "Coche cette case puis avec whitepoint ci-dessous \"pick\" une zone dans les \"high values\" \nPuis décoche la case!" +STARTLINE}
    addUserKnob {41 whitepoint l Pick_whitepoint T Grade_WhitePoint_GreenScreen.whitepoint}
    addUserKnob {26 DespillCtrl}
    addUserKnob {7 supp l Suppression t "Bias the mix of channels used to cap the 'spill' channel.\nFor example when removing 'green spill':\n0 = green channel is capped by the red channel.\n\n0.5 = green channel is capped by half-mix of red and blue channels.\n\n1 = green channel is capped by the blue channel." R 0 2}
    supp 0.5
    addUserKnob {7 masterMult l Limiter t "Gain applied to the 'cap' channel to make the suppression more or less aggressive." R 0 10}
    masterMult 1.1
    addUserKnob {6 ShowDespillCtrl +STARTLINE}
    addUserKnob {26 ""}
    addUserKnob {20 LUMA_CTRL}
    addUserKnob {26 ""}
    addUserKnob {26 DividerPart2 l "" +STARTLINE T "GainMult BG"}
    addUserKnob {26 unnamed l INVISIBLE +INVISIBLE T "Copyright Alexandre Ribeiro"}
    addUserKnob {41 BG_value T Multiply4.value}
    addUserKnob {26 ""}
    addUserKnob {20 BlackCtrl n 1}
    addUserKnob {41 which l MasterBlackIntensity T Dissolve1.which}
    addUserKnob {26 Mask -STARTLINE T ""}
    addUserKnob {6 Show_Mask_BlackCtrl -STARTLINE}
    addUserKnob {41 center l MaskCenterContrast T RolloffContrast2.center}
    addUserKnob {41 contrast l MaskContrast -STARTLINE T RolloffContrast2.contrast}
    addUserKnob {41 value l MaskGain T Multiply8.value}
    addUserKnob {20 OverrideMaskFromDespill n 1}
    addUserKnob {6 OverideMask l ActiveOverideMask +STARTLINE}
    addUserKnob {26 ___optional l "" -STARTLINE T ---optional}
    addUserKnob {7 SuppressionOveride l Suppression R 0 2}
    SuppressionOveride 0.7
    addUserKnob {7 LimiterOveride l Limiter R 0 10}
    LimiterOveride 1.1
    addUserKnob {6 ShowDespillCtrlOverideMask +STARTLINE}
    addUserKnob {20 endGroup_1 l endGroup n -1}
    addUserKnob {20 endGroup n -1}
    addUserKnob {20 MIN_SAT_VALUES}
    addUserKnob {26 ""}
    addUserKnob {41 which_1 l MergeMinValue T Dissolve2.which}
    addUserKnob {26 MinMaskCtrl l "" +STARTLINE T ---Mask}
    addUserKnob {6 ShowMinMask +STARTLINE}
    addUserKnob {41 center_1 l center T RolloffContrast1.center}
    addUserKnob {41 contrast_1 l contrast -STARTLINE T RolloffContrast1.contrast}
    addUserKnob {41 range T Keyer1.range}
    }
    Input {
    inputs 0
    name img
    xpos -872
    ypos -350
    }
    Dot {
    name Dot3
    xpos -838
    ypos -140
    }
    set Nc8ab2000 [stack 0]
    Dot {
    name Dot23
    tile_color 0x8ce08ff
    xpos -192
    ypos -140
    }
    set Nb8142cd0 [stack 0]
    Dot {
    name Dot20
    tile_color 0x8ce08ff
    xpos 629
    ypos -140
    }
    set N31dd4fb0 [stack 0]
    push $N31dd4fb0
    add_layer {rgb_channel2 rgb_channel2.red rgb_channel2.green rgb_channel2.blue}
    ShuffleCopy {
    inputs 2
    red red
    green green
    blue blue
    black red
    white green
    red2 blue
    out2 rgb_channel2
    name ShuffleCopy5
    xpos 595
    ypos 1314
    }
    push $Nc8ab2000
    Dot {
    name Dot22
    tile_color 0x8ce08ff
    xpos -838
    ypos 1007
    }
    set N3b1fe6a0 [stack 0]
    Dot {
    name Dot11
    tile_color 0x8ce08ff
    xpos -683
    ypos 1007
    }
    push $Nb8142cd0
    Shuffle {
    red blue
    green blue
    alpha black
    name Blue
    xpos -82
    ypos -4
    }
    set N386aed60 [stack 0]
    push $Nb8142cd0
    Shuffle {
    red green
    blue green
    alpha black
    name Green
    xpos -226
    ypos -5
    }
    set Nc840adf0 [stack 0]
    push $Nb8142cd0
    Shuffle {
    green red
    blue red
    alpha black
    name Red
    xpos -358
    ypos -4
    }
    set Nc84114d0 [stack 0]
    Switch {
    inputs 3
    which {{back_color}}
    name High
    xpos -358
    ypos 101
    }
    set N6351f8f0 [stack 0]
    push $N6351f8f0
    ShuffleCopy {
    inputs 2
    red red
    green green
    blue blue
    black red
    white green
    red2 blue
    out2 rgb_channel2
    name ShuffleCopy4
    xpos -358
    ypos 180
    }
    push $Nc840adf0
    push $Nc84114d0
    Switch {
    inputs 2
    which {{"back_color == 0 ? 1 : back_color == 1 ? 0 : 0"}}
    name LowA
    xpos -226
    ypos 102
    }
    set N3c6712d0 [stack 0]
    push $N3c6712d0
    ShuffleCopy {
    inputs 2
    red red
    green green
    blue blue
    out2 rgb_channel2
    name ShuffleCopy3
    xpos -226
    ypos 179
    }
    Multiply {
    channels rgb
    value {{"1 - supp"}}
    name Multiply1
    xpos -226
    ypos 275
    }
    Multiply {
    channels rgb_channel2
    value {{parent.SuppressionOveride}}
    name Multiply5
    xpos -226
    ypos 327
    }
    push $N386aed60
    push $Nc840adf0
    push 0
    Switch {
    inputs 3
    which {{"back_color == 0 ? 2 : back_color == 1 ? 2 : 1" i}}
    name LowB
    xpos -82
    ypos 99
    }
    set Nb6127750 [stack 0]
    push $Nb6127750
    ShuffleCopy {
    inputs 2
    red red
    green green
    blue blue
    out2 rgb_channel2
    name ShuffleCopy2
    xpos -82
    ypos 178
    }
    Multiply {
    channels rgb
    value {{supp}}
    name Multiply2
    xpos -82
    ypos 275
    }
    Multiply {
    channels rgb_channel2
    value {{parent.SuppressionOveride}}
    name Multiply6
    xpos -82
    ypos 329
    }
    Merge2 {
    inputs 2
    operation plus
    also_merge rgb_channel2
    name Merge1
    xpos -151
    ypos 408
    }
    Multiply {
    channels rgb
    value {{masterMult}}
    name Multiply3
    xpos -151
    ypos 474
    }
    Multiply {
    channels rgb_channel2
    value {{parent.LimiterOveride}}
    name Multiply7
    xpos -151
    ypos 515
    }
    Dot {
    name Dot8
    tile_color 0x8ce08ff
    xpos -117
    ypos 935
    }
    Merge2 {
    inputs 2
    operation min
    also_merge rgb_channel2
    name Merge10
    xpos -358
    ypos 931
    }
    Dot {
    name Dot31
    tile_color 0x8ce08ff
    xpos -324
    ypos 1072
    }
    Dot {
    name Dot25
    tile_color 0x8ce08ff
    xpos -599
    ypos 1072
    }
    set N33ff8bd0 [stack 0]
    Dot {
    name Dot32
    tile_color 0x8ce08ff
    xpos -599
    ypos 1293
    }
    set N63ff6dd0 [stack 0]
    ShuffleCopy {
    inputs 2
    red red
    green green
    name ShuffleBlue
    xpos -717
    ypos 1289
    }
    push $N63ff6dd0
    Dot {
    name Dot26
    tile_color 0x8ce08ff
    xpos -599
    ypos 1351
    }
    ShuffleCopy {
    inputs 2
    in2 rgb_channel2
    red red
    green green
    blue blue
    black red
    white green
    out2 rgb_channel2
    name ShuffleBlue1
    xpos -717
    ypos 1347
    }
    Dot {
    name Dot36
    tile_color 0x8ce08ff
    xpos -683
    ypos 1414
    }
    push $N3b1fe6a0
    push $N33ff8bd0
    Dot {
    name Dot30
    tile_color 0x8ce08ff
    xpos -762
    ypos 1151
    }
    set N637f9be0 [stack 0]
    ShuffleCopy {
    inputs 2
    red red
    blue blue
    name ShuffleGreen
    xpos -872
    ypos 1147
    }
    push $N637f9be0
    Dot {
    name Dot29
    tile_color 0x8ce08ff
    xpos -762
    ypos 1217
    }
    ShuffleCopy {
    inputs 2
    in2 rgb_channel2
    red red
    green green
    blue blue
    black red
    red2 blue
    out2 rgb_channel2
    name ShuffleGreen1
    xpos -872
    ypos 1213
    }
    push $N3b1fe6a0
    Dot {
    name Dot24
    tile_color 0x8ce08ff
    xpos -1047
    ypos 1007
    }
    push $N33ff8bd0
    Dot {
    name Dot27
    tile_color 0x8ce08ff
    xpos -952
    ypos 1072
    }
    set N326a8490 [stack 0]
    ShuffleCopy {
    inputs 2
    green green
    blue blue
    name ShuffleRed
    xpos -1081
    ypos 1068
    }
    push $N326a8490
    Dot {
    name Dot28
    tile_color 0x8ce08ff
    xpos -952
    ypos 1121
    }
    ShuffleCopy {
    inputs 2
    in2 rgb_channel2
    red red
    green green
    blue blue
    white green
    red2 blue
    out2 rgb_channel2
    name ShuffleRed1
    xpos -1081
    ypos 1117
    }
    Dot {
    name Dot33
    tile_color 0x8ce08ff
    xpos -1047
    ypos 1414
    }
    Switch {
    inputs 3
    which {{back_color}}
    name Switch1
    xpos -872
    ypos 1410
    }
    Dot {
    name Dot34
    tile_color 0x8ce08ff
    xpos -838
    ypos 1542
    }
    set N70bc73a0 [stack 0]
    Merge2 {
    inputs 2
    operation minus
    also_merge rgb_channel2
    name Merge9
    xpos 595
    ypos 1538
    }
    set N73a1bd10 [stack 0]
    Grade {
    channels all
    whitepoint {0 1 0 0}
    name Grade_WhitePoint_GreenScreen
    xpos 595
    ypos 1951
    }
    Saturation {
    channels all
    saturation 0
    mode Maximum
    name Saturation1
    xpos 595
    ypos 2091
    }
    set Nc1f62720 [stack 0]
    Shuffle {
    in rgb_channel2
    name RGB_Channel2_1
    label "\[value in]"
    xpos 374
    ypos 2149
    }
    push $Nc1f62720
    Shuffle {
    name RGB
    label "\[value in]"
    xpos 371
    ypos 2045
    }
    Switch {
    inputs 2
    which {{parent.OverideMask}}
    name Switch2
    xpos 206
    ypos 2097
    }
    Shuffle {
    alpha red
    name Shuffle2
    label "\[value in]"
    xpos -208
    ypos 2091
    }
    set N251fb000 [stack 0]
    Input {
    inputs 0
    name BG
    xpos 1026
    ypos -309
    number 2
    }
    Dot {
    name Dot12
    tile_color 0x8ce08ff
    xpos 1060
    ypos 2321
    }
    set N9580b90 [stack 0]
    Dot {
    name Dot13
    tile_color 0x8ce08ff
    xpos 241
    ypos 2321
    }
    set Nb6310580 [stack 0]
    Merge2 {
    inputs 2
    operation multiply
    name Merge3
    xpos 46
    ypos 2317
    disable true
    }
    push $Nb6310580
    Dot {
    name Dot14
    tile_color 0x8ce08ff
    xpos 241
    ypos 2586
    }
    set N385b4ab0 [stack 0]
    Keyer {
    invert true
    operation "luminance key"
    range {0.08061033812 0.3427249312 1 1}
    name Keyer1
    xpos -26
    ypos 2576
    }
    push $N251fb000
    RolloffContrast {
    contrast 1.5
    slope_mag_low1 0.8000000119
    black_low 1
    slope_mag_high2 0.8000000119
    white_high 1
    name RolloffContrast1
    xpos -208
    ypos 2464
    }
    ChannelMerge {
    inputs 2
    operation min
    name ChannelMerge1
    xpos -208
    ypos 2569
    }
    set Nb812f1b0 [stack 0]
    Shuffle {
    in alpha
    name Shuffle5
    label "\[value in]"
    xpos -208
    ypos 2756
    }
    Dot {
    name Dot42
    tile_color 0xd11700ff
    label "Link to: <font size = 3 color=\"green\"> \[value input.name]"
    xpos -126
    ypos 4283
    hide_input true
    }
    push $N73a1bd10
    Dot {
    name Dot15
    tile_color 0xd11700ff
    label "Link to: <font size = 3 color=\"green\"> \[value input.name]"
    xpos -244
    ypos 4231
    hide_input true
    }
    push $N251fb000
    RolloffContrast {
    channels alpha
    contrast 2
    center 1
    slope_mag_low1 0.8000000119
    black_low 1
    slope_mag_high2 0.8000000119
    white_high 1
    name RolloffContrast2
    xpos -376
    ypos 2091
    }
    Multiply {
    channels alpha
    value 1.1
    name Multiply8
    xpos -486
    ypos 2091
    }
    Clamp {
    channels alpha
    name Clamp1
    xpos -596
    ypos 2091
    }
    set N387a7100 [stack 0]
    Shuffle {
    in alpha
    name Shuffle4
    label "\[value in]"
    xpos -596
    ypos 2180
    }
    Dot {
    name Dot7
    tile_color 0xd11700ff
    label "Link to: <font size = 3 color=\"green\"> \[value input.name]"
    xpos -342
    ypos 4180
    hide_input true
    }
    push $Nc1f62720
    Shuffle {
    in rgb_channel2
    name Shuffle3
    label "\[value in]"
    xpos 708
    ypos 2175
    }
    Dot {
    name Dot41
    tile_color 0xd11700ff
    label "Link to: <font size = 3 color=\"green\"> \[value input.name]"
    xpos -445
    ypos 4132
    hide_input true
    }
    push $Nc1f62720
    Dot {
    name Dot40
    tile_color 0xd11700ff
    label "Link to: <font size = 3 color=\"green\"> \[value input.name]"
    xpos -565
    ypos 4084
    hide_input true
    }
    Input {
    inputs 0
    name mask
    xpos -616
    ypos 3885
    number 1
    }
    push $Nc8ab2000
    Dot {
    name Dot2
    xpos -1268
    ypos -140
    }
    Dot {
    name Dot21
    tile_color 0x8ce08ff
    xpos -1268
    ypos 3660
    }
    set N261ebfe0 [stack 0]
    Dot {
    name Dot4
    tile_color 0x8ce08ff
    xpos -1268
    ypos 3889
    }
    push $N261ebfe0
    push $Nb812f1b0
    Dot {
    name Dot35
    tile_color 0x8ce08ff
    xpos -433
    ypos 2586
    }
    push $N385b4ab0
    Dot {
    name Dot19
    tile_color 0x8ce08ff
    xpos 241
    ypos 2665
    }
    push $N387a7100
    push $N70bc73a0
    Dot {
    name Dot18
    tile_color 0x8ce08ff
    xpos -838
    ypos 2026
    }
    set N7385b360 [stack 0]
    Dot {
    name Dot17
    tile_color 0x8ce08ff
    xpos -719
    ypos 2026
    }
    Grade {
    inputs 1+1
    white 0
    name Grade4
    xpos -753
    ypos 2097
    }
    Dot {
    name Dot16
    tile_color 0x8ce08ff
    xpos -719
    ypos 2178
    }
    push $N7385b360
    Dissolve {
    inputs 2
    which 1
    name Dissolve1
    xpos -872
    ypos 2168
    }
    Dot {
    name Dot37
    tile_color 0x8ce08ff
    xpos -838
    ypos 2581
    }
    set N8c4ea280 [stack 0]
    Dot {
    name Dot39
    tile_color 0x8ce08ff
    xpos -702
    ypos 2581
    }
    Merge2 {
    inputs 2+1
    operation min
    name Merge4
    xpos -736
    ypos 2661
    }
    Dot {
    name Dot38
    tile_color 0x8ce08ff
    xpos -702
    ypos 2746
    }
    push $N8c4ea280
    Dissolve {
    inputs 2
    which 1
    name Dissolve2
    xpos -872
    ypos 2736
    }
    push $Nc1f62720
    Dot {
    name Dot9
    tile_color 0x8ce08ff
    xpos 629
    ypos 2776
    }
    set Nb6780f50 [stack 0]
    Dot {
    name Dot1
    tile_color 0x8ce08ff
    xpos 796
    ypos 2776
    }
    push $N9580b90
    Colorspace {
    colorspace_in sRGB
    colorspace_out HSV
    name Colorspace1
    xpos 1026
    ypos 2861
    }
    Dot {
    name Dot5
    tile_color 0x8ce08ff
    xpos 1060
    ypos 2917
    }
    set N635b2b90 [stack 0]
    Dot {
    name Dot6
    tile_color 0x8ce08ff
    xpos 1060
    ypos 2955
    }
    set N323aa3d0 [stack 0]
    Grade {
    inputs 1+1
    white 0
    maskChannelMask rgba.blue
    invert_mask true
    name Grade2
    xpos 1026
    ypos 3054
    }
    Shuffle {
    red blue
    green blue
    alpha blue
    name Shuffle1
    label "\[value in]"
    xpos 1026
    ypos 3099
    }
    Multiply {
    channels rgb
    name Multiply4
    xpos 1026
    ypos 3207
    }
    push $N323aa3d0
    push $N635b2b90
    push $Nb6780f50
    Colorspace {
    colorspace_in sRGB
    colorspace_out HSV
    name Colorspace4
    xpos 595
    ypos 2858
    }
    Copy {
    inputs 2
    from0 rgba.red
    to0 rgba.red
    name Copy1
    xpos 595
    ypos 2907
    }
    Copy {
    inputs 2
    from0 rgba.green
    to0 rgba.green
    name Copy2
    xpos 595
    ypos 2945
    }
    Colorspace {
    colorspace_in HSV
    colorspace_out sRGB
    name Colorspace3
    xpos 595
    ypos 3110
    }
    Merge2 {
    inputs 2
    operation multiply
    name Merge2
    xpos 595
    ypos 3207
    }
    Dot {
    name Dot10
    tile_color 0x8ce08ff
    xpos 629
    ypos 3421
    }
    Merge2 {
    inputs 2
    operation plus
    name Merge11
    xpos -872
    ypos 3417
    }
    ShuffleCopy {
    inputs 2
    name ShuffleCopy1
    xpos -872
    ypos 3656
    }
    Keymix {
    inputs 3
    invertMask true
    bbox A
    name Keymix1
    xpos -872
    ypos 3879
    disable {{"\[exists parent.input1] ? 0 : 1" x76 0}}
    }
    Switch {
    inputs 2
    which {{parent.ShowDespillCtrl}}
    name Switch_RGB1_LIMITE
    xpos -872
    ypos 4080
    }
    Switch {
    inputs 2
    which {{parent.ShowDespillCtrlOverideMask}}
    name Switch_RGB2_LIMITE
    xpos -872
    ypos 4128
    }
    Switch {
    inputs 2
    which {{parent.Show_Mask_BlackCtrl}}
    name Switch_Show_BlackCtrl
    xpos -872
    ypos 4176
    }
    Switch {
    inputs 2
    which {{parent.Show_WhitePoint}}
    name Switch_WhitePointCtrl
    xpos -872
    ypos 4227
    }
    Switch {
    inputs 2
    which {{parent.ShowMinMask}}
    name Switch_MinMaskCtrl
    xpos -872
    ypos 4279
    }
    Output {
    name Output1
    xpos -872
    ypos 4508
    }
    end_group
```
