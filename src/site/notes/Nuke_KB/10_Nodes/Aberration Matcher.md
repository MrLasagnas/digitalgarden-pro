---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/aberration-matcher/","tags":["nuke","node","aberration","lens"]}
---


# Aberration Matcher

## 🧠 Description
Setup to match plate aberration

## 🎯 When to use
- Optical effects
- Plate matching

## ⚠️ Known pitfalls

## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code
```nuke
    set cut_paste_input [stack 0]
    version 13.2 v8
    Roto {
     inputs 0
     output alpha
     cliptype none
     curves {{{v x3f99999a}
      {f 0}
      {n
       {layer Root
        {f 2097152}
        {t x45100000 x4445c000}
        {a pt1x 0 pt1y 0 pt2x 0 pt2y 0 pt3x 0 pt3y 0 pt4x 0 pt4y 0 ptex00 0 ptex01 0 ptex02 0 ptex03 0 ptex10 0 ptex11 0 ptex12 0 ptex13 0 ptex20 0 ptex21 0 ptex22 0 ptex23 0 ptex30 0 ptex31 0 ptex32 0 ptex33 0 ptof1x 0 ptof1y 0 ptof2x 0 ptof2y 0 ptof3x 0 ptof3y 0 ptof4x 0 ptof4y 0 pterr 0 ptrefset 0 ptmot x40800000 ptref 0}}}}}
     toolbox {selectAll {
      { selectAll str 1 ssx 1 ssy 1 sf 1 }
      { createBezier str 1 ssx 1 ssy 1 sf 1 sb 1 tt 4 }
      { createBezierCusped str 1 ssx 1 ssy 1 sf 1 sb 1 tt 5 }
      { createBSpline str 1 ssx 1 ssy 1 sf 1 sb 1 tt 6 }
      { createEllipse str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { createRectangle str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { createRectangleCusped str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { brush str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { eraser src 2 str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { clone src 1 str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { reveal src 3 str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { dodge src 1 str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { burn src 1 str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { blur src 1 str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { sharpen src 1 str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { smear src 1 str 1 ssx 1 ssy 1 sf 1 sb 1 }
    } }
     toolbar_brush_hardness 0.200000003
     toolbar_source_transform_scale {1 1}
     toolbar_source_transform_center {2304 791}
     colorOverlay {0 0 0 0}
     lifetime_type "all frames"
     lifetime_start 7960
     lifetime_end 7960
     view {}
     motionblur_on true
     motionblur_shutter_offset_type centred
     source_black_outside true
     name Roto88
     selected true
     xpos -7131
     ypos -1387
    }
    Dot {
     name Dot139
     selected true
     xpos -7100
     ypos -1299
    }
    push 0
    Dot {
     name Dot155
     label PLATE
     note_font "Bitstream Vera Sans Bold"
     note_font_size 30
     selected true
     xpos -7252
     ypos -1797
    }
    TimeWarp {
     lookup {{t>>2 C x995 995 x1092 1092}}
     time ""
     filter none
     name TimeWarp4
     selected true
     xpos -7283
     ypos -1723
    }
    Dot {
     name Dot152
     selected true
     xpos -7252
     ypos -1667
    }
    set N3cbd7140 [stack 0]
    Dot {
     name Dot153
     selected true
     xpos -7396
     ypos -1667
    }
    set Nea673ef0 [stack 0]
    Dot {
     name Dot154
     selected true
     xpos -7541
     ypos -1667
    }
    Shuffle1 {
     green red
     blue red
     alpha red
     name Shuffle22
     label RED
     selected true
     xpos -7572
     ypos -1578
    }
    Dot {
     name Dot156
     selected true
     xpos -7541
     ypos -1438
    }
    push $Nea673ef0
    Shuffle1 {
     red green
     blue green
     alpha green
     name Shuffle8
     label GREEN
     selected true
     xpos -7427
     ypos -1578
    }
    Dot {
     name Dot157
     selected true
     xpos -7396
     ypos -1497
    }
    push $N3cbd7140
    Shuffle1 {
     red blue
     green blue
     alpha blue
     name Shuffle9
     label BLUE
     selected true
     xpos -7286
     ypos -1595
    }
    Switch {
     inputs 2
     which {{t%2}}
     name Switch1
     selected true
     xpos -7286
     ypos -1498
    }
    Switch {
     inputs 2
     which {{!(t%4)}}
     name Switch5
     selected true
     xpos -7286
     ypos -1439
    }
    VectorGenerator {
     motionEstimation Regularized
     vectorDetailReg 1
     strengthReg 1
     Advanced 1
     flickerCompensation true
     name VectorGenerator4
     selected true
     xpos -7286
     ypos -1379
    }
    Multiply {
     inputs 1+1
     channels motion
     value 0.04
     name Multiply6
     selected true
     xpos -7286
     ypos -1306
     disable true
    }
    Blur {
     channels motion
     size 100
     name Blur77
     label "\[value size]"
     selected true
     xpos -7286
     ypos -1248
    }
    Dot {
     name Dot165
     selected true
     xpos -7255
     ypos -1171
    }
    push $cut_paste_input
    Dot {
     name Dot160
     label CG
     note_font "Bitstream Vera Sans Bold"
     note_font_size 30
     selected true
     xpos -6773
     ypos -1391
    }
    set Neb5bfc40 [stack 0]
    Dot {
     name Dot135
     selected true
     xpos -6939
     ypos -1394
    }
    NoTimeBlur {
     name NoTimeBlur1
     selected true
     xpos -6970
     ypos -1320
     disable true
    }
    TimeWarp {
     lookup {{t>>2 C x995 995 x1092 1092}}
     time ""
     filter none
     name TimeWarp5
     selected true
     xpos -6970
     ypos -1268
    }
    Dot {
     name Dot138
     selected true
     xpos -6939
     ypos -1233
    }
    Dot {
     name Dot163
     selected true
     xpos -7080
     ypos -1233
    }
    ShuffleCopy {
     inputs 2
     in motion
     red red
     green green
     blue blue
     out motion
     name ShuffleCopy8
     selected true
     xpos -7111
     ypos -1172
    }
    IDistort {
     channels rgba
     uv motion
     name IDistort6
     selected true
     xpos -7114
     ypos -1096
    }
    set Neb5e4b10 [stack 0]
    Dot {
     name Dot140
     selected true
     xpos -7208
     ypos -1095
    }
    TimeOffset {
     time_offset -2
     time ""
     name TimeOffset3
     label "\[value time_offset]"
     selected true
     xpos -7239
     ypos -1052
    }
    TimeWarp {
     lookup {{t<<2 C x995 995 x1092 1092}}
     time ""
     filter none
     name TimeWarp8
     selected true
     xpos -7239
     ypos -999
    }
    Dot {
     name Dot159
     selected true
     xpos -7208
     ypos -897
    }
    push $Neb5e4b10
    TimeWarp {
     lookup {{t<<2 C x995 995 x1092 1092}}
     time ""
     filter none
     name TimeWarp3
     selected true
     xpos -7114
     ypos -1046
    }
    Dot {
     name Dot141
     selected true
     xpos -7083
     ypos -994
    }
    push $Neb5bfc40
    Copy {
     inputs 2
     from0 rgba.red
     to0 rgba.red
     bbox B
     name Copy7
     selected true
     xpos -6804
     ypos -1001
    }
    Copy {
     inputs 2
     from0 rgba.blue
     to0 rgba.blue
     bbox B
     name Copy8
     selected true
     xpos -6804
     ypos -904
    }
    StickyNote {
     inputs 0
     name StickyNote17
     tile_color 0xa39934ff
     label "Vectors adjustments\n"
     note_font Verdana
     note_font_size 15
     selected true
     xpos -7467
     ypos -1318
    }
```
