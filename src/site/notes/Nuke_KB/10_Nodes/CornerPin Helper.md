---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/corner-pin-helper/","tags":["nuke","node","cornerpin"]}
---


# CornerPin Helper

## 🧠 Description


## 🎯 When to use


## ⚠️ Known pitfalls


## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```nuke
    set cut_paste_input [stack 0]
    version 14.0 v2
    push $cut_paste_input
    Group {
     name CornerPinHelper
     help "Connect this node directly to your CornerPin node in order to see helper lines between the CornerPin's points - which can help with lining up the perspective on for example screen replacements. "
     selected true
     xpos -103
     ypos -137
     addUserKnob {20 User l Settings}
     addUserKnob {6 lineColour_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
     addUserKnob {7 lineWidth l "Line Width" R 0 25}
     lineWidth 10
     addUserKnob {7 lineSpacing l "Line Spacing" R 0 10}
     lineSpacing 0.05
     addUserKnob {19 lineColour l "Line Colour"}
     lineColour {1 0 1 0.5}
     addUserKnob {6 lineColour_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
     addUserKnob {26 ""}
     addUserKnob {26 author l "" +STARTLINE T "Kenn Hedin Kalvik"}
     addUserKnob {26 version l "" +STARTLINE T "CornerPin Helper v1.0 | 2025"}
     addUserKnob {26 website l "" +STARTLINE T "<a href='https://www.keheka.com'>www.keheka.com<a>"}
    }
     Input {
      inputs 0
      name InputCornerPin
      xpos -189
      ypos -232
     }
     Dot {
      name Dot1
      xpos -155
      ypos -148
     }
    set N10d25800 [stack 0]
     Dot {
      name Dot2
      xpos -278
      ypos -148
     }
     Remove {
      name Remove1
      xpos -312
      ypos -96
     }
     RotoPaint {
      curves {{{v x3f99999a}
      {f 0}
      {n
       {layer Root
        {f 2097664}
        {t x4506c000 x4497a000}
        {a pt1x 0 pt1y 0 pt2x 0 pt2y 0 pt3x 0 pt3y 0 pt4x 0 pt4y 0 ptex00 0 ptex01 0 ptex02 0 ptex03 0 ptex10 0 ptex11 0 ptex12 0 ptex13 0 ptex20 0 ptex21 0 ptex22 0 ptex23 0 ptex30 0 ptex31 0 ptex32 0 ptex33 0 ptof1x 0 ptof1y 0 ptof2x 0 ptof2y 0 ptof3x 0 ptof3y 0 ptof4x 0 ptof4y 0 pterr 0 ptrefset 0 ptmot x40800000 ptref 0}
        {cubiccurve Brush4 512 catmullrom
         {cc
          {f 2080}
          {p
           {{=parent.input.to4.x x44e30000}
            {=parent.input.to4.y x44670000}       1}
           {{=parent.input.to1.x x44e60000}
            {=parent.input.to1.y x446b0000}       1}}}
         {tx x447a4000 x44e4d000 x4468e000}
         {a ro 0 go 0 bo 0 ao 0 bs
          {=parent.lineWidth x41c80000}     bsp
       {"=clamp(parent.lineSpacing,\ 0.05,\ 10000)"
        {{0 x3d4ccccd -}}}     h 1 bu 1 str 1 spx x4506c000 spy x4497a000 sb 1 ltn x447a4000 ltm x447a4000 tt x41880000}}
        {cubiccurve Brush3 512 catmullrom
         {cc
          {f 2080}
          {p
           {{=parent.input.to3.x x44e30000}
            {=parent.input.to3.y x44670000}       1}
           {{=parent.input.to4.x x44e60000}
            {=parent.input.to4.y x446b0000}       1}}}
         {tx x447a4000 x44e4d000 x4468e000}
         {a ro 0 go 0 bo 0 ao 0 bs
          {=parent.lineWidth x41c80000}     bsp
       {"=clamp(parent.lineSpacing,\ 0.05,\ 10000)"
        {{0 x3d4ccccd -}}}     h 1 bu 1 str 1 spx x4506c000 spy x4497a000 sb 1 ltn x447a4000 ltm x447a4000 tt x41880000}}
        {cubiccurve Brush2 512 catmullrom
         {cc
          {f 2080}
          {p
           {{=parent.input.to2.x 0}
            {=parent.input.to2.y 0}       1}
           {{=parent.input.to3.x 0}
            {=parent.input.to3.y 0}       1}}}
         {tx x447a4000 x44e4d000 x4468e000}
         {a ro 0 go 0 bo 0 ao 0 bu 1 str 1 spx x4506c000 spy x4497a000 sb 1 ltn x447a4000 ltm x447a4000 tt x41880000 h 1 bs
          {=parent.lineWidth x41c80000}     bsp
       {"=clamp(parent.lineSpacing,\ 0.05,\ 10000)"
        {{0 x3d4ccccd -}}}}}
        {cubiccurve Brush1 512 catmullrom
         {cc
          {f 2080}
          {p
           {{=parent.input.to1.x x44e30000}
            {=parent.input.to1.y x44670000}       1}
           {{=parent.input.to2.x x44e60000}
            {=parent.input.to2.y x446b0000}       1}}}
         {tx x447a4000 x44e4d000 x4468e000}
         {a ro 0 go 0 bo 0 ao 0 bs
          {=parent.lineWidth x41c80000}     bsp
       {"=clamp(parent.lineSpacing,\ 0.05,\ 10000)"
        {{0 x3d4ccccd -}}}     h 1 bu 1 str 1 spx x4506c000 spy x4497a000 sb 1 ltn x447a4000 ltm x447a4000 tt x41880000}}}}}}
      toolbox {selectAll {
      { selectAll str 1 ssx 1 ssy 1 sf 1 }
      { createBezier str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { createBezierCusped str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { createBSpline str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { createEllipse str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { createRectangle str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { createRectangleCusped str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { brush str 1 ssx 1 ssy 1 sf 1 sb 1 ltn 1001 ltm 1001 tt 17 }
      { eraser src 2 str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { clone src 1 str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { reveal src 3 str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { dodge src 1 str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { burn src 1 str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { blur src 1 str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { sharpen src 1 str 1 ssx 1 ssy 1 sf 1 sb 1 }
      { smear src 1 str 1 ssx 1 ssy 1 sf 1 sb 1 }
    } }
      toolbar_label_points true
      toolbar_show_paint_selection true
      toolbar_brush_hardness 0.200000003
      toolbar_lifetime_type single
      toolbar_source_transform_scale {1 1}
      toolbar_source_transform_center {2156 1213}
      colorOverlay {0 0 0 0}
      lifetime_type "all frames"
      lifetime_start 1001
      lifetime_end 1001
      motionblur_on true
      brush_size {{parent.lineWidth 25}}
      brush_spacing {{"clamp(parent.lineSpacing, 0.05, 10000)" 0.05000000075}}
      brush_hardness 1
      source_black_outside true
      name RotoPaint1
      xpos -312
      ypos -22
     }
     Multiply {
      channels rgb
      value {{parent.lineColour.r} {parent.lineColour.g} {parent.lineColour.b} 1}
      unpremult rgba.alpha
      name Multiply1
      xpos -312
      ypos 34
     }
     Multiply {
      value {{parent.lineColour.a}}
      name Multiply2
      xpos -312
      ypos 99
     }
     Dot {
      name Dot3
      xpos -278
      ypos 173
     }
    push $N10d25800
     Merge2 {
      inputs 2
      name Merge1
      xpos -189
      ypos 169
     }
     Output {
      name Output1
      xpos -189
      ypos 269
     }
    end_group
```
