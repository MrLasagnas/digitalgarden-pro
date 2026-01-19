---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/additive-keyer/","tags":["nuke","node","keying"]}
---


# AdditiveKeyer

## 🧠 Description
Little group node for additive keying.

## 🎯 When to use


## ⚠️ Known pitfalls


## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code
```nuke
    set cut_paste_input [stack 0]
    version 14.0 v5
    push 0
    push 0
    push 0
    push 0
    push $cut_paste_input
    Group {
     inputs 5
     name AdditiveKeyer_HB
     tile_color 0x7247ff
     gl_color 0xffffffff
     note_font "Segoe MDL2 Assets Bold"
     note_font_size 20
     note_font_color 0xffffffff
     selected true
     xpos 4173
     ypos 635
     addUserKnob {20 additivekeyer l "Additive Keyer"}
     addUserKnob {26 _1 l "" +STARTLINE}
     addUserKnob {6 disabledarkvalues l "Disable Dark Values" +STARTLINE}
     addUserKnob {7 darkvalues l "Dark Value" R 0 5}
     darkvalues 1
     addUserKnob {7 darksat l "Dark Sat"}
     darksat 1
     addUserKnob {26 _2 l "Dark Mask" T ""}
     addUserKnob {41 maskChannelMask l "" -STARTLINE T Merge6.maskChannelMask}
     addUserKnob {41 inject_1 l inject T Merge6.inject}
     addUserKnob {41 invert_mask_1 l invert -STARTLINE T Merge6.invert_mask}
     addUserKnob {41 fringe_1 l fringe -STARTLINE T Merge6.fringe}
     addUserKnob {26 _4 l "" +STARTLINE}
     addUserKnob {6 disablebrightvalues l "Disable Bright Values" +STARTLINE}
     addUserKnob {7 brightvalue l "Bright Value" R 0 5}
     brightvalue 1
     addUserKnob {7 brightsat l "Bright Sat"}
     brightsat 1
     addUserKnob {26 _3 l "Bright Mask" T ""}
     addUserKnob {41 maskChannelMask_1 l "" -STARTLINE T Merge7.maskChannelMask}
     addUserKnob {41 inject T Merge7.inject}
     addUserKnob {41 invert_mask l invert -STARTLINE T Merge7.invert_mask}
     addUserKnob {41 fringe -STARTLINE T Merge7.fringe}
     addUserKnob {26 global l Global}
     addUserKnob {7 mix l Mix}
     mix 1
     addUserKnob {6 logspace l "Work in Logspace ?" +STARTLINE}
     logspace true
     addUserKnob {20 tab l Info}
     addUserKnob {26 text l "" +STARTLINE T "By Hugo Baptista\nv1.0\n\nInspired by Sebastian Schutt & Tony Lyons"}
    }
     Input {
      inputs 0
      name BG
      xpos 559
      ypos -2860
     }
     AddChannels {
      channels rgba
      name AddChannels3
      xpos 559
      ypos -2786
     }
     Log2Lin {
      operation lin2log
      name Log2Lin3
      xpos 559
      ypos -2673
      disable {{"1 - parent.logspace"}}
     }
     Dot {
      name Dot1
      xpos 590
      ypos -2081
     }
    set Nc514c00 [stack 0]
     Dot {
      name Dot2
      xpos 1641
      ypos -2081
     }
     Dot {
      name Dot3
      xpos 1641
      ypos 65
     }
    set Nc515400 [stack 0]
     Dot {
      name Dot4
      xpos 1641
      ypos 361
     }
     Input {
      inputs 0
      name BrightMask
      xpos 889
      ypos -823
      number 4
     }
     Dot {
      name Dot34
      label "BRIGHT MASK"
      xpos 920
      ypos -648
     }
     Dot {
      name Dot35
      xpos 920
      ypos -231
     }
     Input {
      inputs 0
      name FG
      xpos -761
      ypos -3043
      number 1
     }
     Remove {
      channels alpha
      name Remove2
      xpos -761
      ypos -2919
     }
     Log2Lin {
      operation lin2log
      name Log2Lin1
      xpos -761
      ypos -2673
      disable {{"1 - parent.logspace"}}
     }
     Dot {
      name Dot12
      xpos -730
      ypos -2572
     }
    set Nc539400 [stack 0]
     Dot {
      name Dot15
      xpos -290
      ypos -2572
     }
     Dot {
      name Dot16
      xpos -290
      ypos -2276
     }
    set Nc539c00 [stack 0]
     OFXuk.co.thefoundry.keylight.keylight_v201 {
      show "Final Result"
      unPreMultiply false
      screenColour {0.3172701001 0.6568404436 0.3367830217}
      screenGain 1
      screenBalance 0.5
      alphaBias {0.5 0.5 0.5}
      despillBias {0.5 0.5 0.5}
      gangBiases true
      preBlur 0
      "Screen Matte" 0
      screenClipMin 0
      screenClipMax 1
      screenClipRollback 0
      screenGrowShrink 0
      screenSoftness 0
      screenDespotBlack 0
      screenDespotWhite 0
      screenReplaceMethod "Soft Colour"
      screenReplaceColour {0.5 0.5 0.5}
      Tuning 0
      midPoint 0.5
      lowGain 1
      midGain 1
      highGain 1
      "Inside Mask" 0
      sourceAlphaHandling Ignore
      insideReplaceMethod "Soft Colour"
      insideReplaceColour {0.5 0.5 0.5}
      Crops 0
      SourceXMethod Colour
      SourceYMethod Colour
      SourceEdgeColour 0
      SourceCropL 0
      SourceCropR 1
      SourceCropB 0
      SourceCropT 1
      balanceSet false
      insideComponent None
      outsideComponent None
      cacheBreaker true
      name Keylight2
      xpos -321
      ypos -2202
     }
    set Nc55c000 [stack 0]
    push $Nc539c00
     Dot {
      name Dot27
      xpos -119
      ypos -2276
     }
     Merge2 {
      inputs 2
      operation from
      name Merge13
      xpos -150
      ypos -2202
     }
     Clamp {
      channels rgb
      name Clamp1
      xpos -150
      ypos -2171
     }
     Saturation {
      saturation 0
      name Saturation5
      xpos -150
      ypos -2128
     }
    push $Nc55c000
     Merge2 {
      inputs 2
      operation plus
      name Merge14
      xpos -321
      ypos -2128
     }
     Dot {
      name Dot5
      xpos -290
      ypos -2081
     }
    set Nc55d800 [stack 0]
     Saturation {
      saturation {{parent.brightsat}}
      name SatBright
      xpos -321
      ypos -2007
     }
     Dot {
      name Dot10
      xpos -290
      ypos -1933
     }
    push $Nc539400
     Saturation {
      saturation 0
      name Saturation3
      xpos -761
      ypos -2498
     }
     Dot {
      name Dot17
      xpos -730
      ypos -2350
     }
    set Nbcd8800 [stack 0]
     Dot {
      name Dot7
      xpos 67
      ypos -2350
     }
    set Nbcd8c00 [stack 0]
     Dot {
      name Dot22
      xpos 260
      ypos -2350
     }
     Merge2 {
      inputs 2
      operation divide
      name Merge2
      xpos 229
      ypos -1933
     }
     Dot {
      name Dot25
      xpos 260
      ypos -648
     }
     Input {
      inputs 0
      name CleanPlate
      xpos -981
      ypos -3043
      number 2
     }
     Remove {
      channels alpha
      name Remove1
      xpos -981
      ypos -2922
     }
     Log2Lin {
      operation lin2log
      name Log2Lin7
      xpos -981
      ypos -2673
      disable {{"1 - parent.logspace"}}
     }
     Saturation {
      saturation 0
      name Saturation4
      xpos -981
      ypos -2498
     }
     Dot {
      name Dot19
      xpos -950
      ypos -2202
     }
    set Nbcfac00 [stack 0]
    push $Nbcd8800
     Merge2 {
      inputs 2
      operation from
      bbox B
      name Merge9
      xpos -761
      ypos -2202
     }
     Clamp {
      name Clamp4
      xpos -761
      ypos -802
     }
     Merge2 {
      inputs 2
      operation multiply
      name Merge12
      xpos -761
      ypos -648
     }
     Clamp {
      channels rgb
      maximum_enable false
      name Clamp5
      xpos -761
      ypos -453
     }
     Multiply {
      channels rgb
      value {{parent.brightvalue}}
      name Multiply2
      xpos -761
      ypos -379
     }
     Dot {
      name Dot26
      xpos -730
      ypos -231
     }
     Input {
      inputs 0
      name DarkMask
      xpos 779
      ypos -1785
      number 3
     }
     Dot {
      name Dot32
      label "DARK MASK"
      xpos 810
      ypos -1686
     }
     Dot {
      name Dot33
      xpos 810
      ypos -1166
     }
    push $Nc55d800
     Dot {
      name Dot6
      xpos -449
      ypos -2081
     }
     Saturation {
      saturation {{parent.darksat}}
      name SatDark
      xpos -480
      ypos -2007
     }
     Dot {
      name Dot8
      xpos -449
      ypos -1859
     }
    push $Nbcd8c00
     Merge2 {
      inputs 2
      operation divide
      name Merge11
      xpos 36
      ypos -1859
     }
     Dot {
      name Dot9
      xpos 67
      ypos -1711
     }
    push $Nbcd8800
     Dot {
      name Dot18
      xpos -1280
      ypos -2350
     }
     Dot {
      name Dot20
      xpos -1280
      ypos -2054
     }
    push $Nbcfac00
     Merge2 {
      inputs 2
      operation divide
      name Merge8
      xpos -981
      ypos -2054
     }
    set Nc3c6800 [stack 0]
     Merge2 {
      inputs 2
      operation multiply
      name Merge10
      xpos -981
      ypos -1711
     }
    set Nc3c7800 [stack 0]
     Clamp {
      channels rgb
      minimum_enable false
      name Clamp2
      xpos -981
      ypos -1563
     }
     Add {
      channels rgba
      value -1
      name Add3
      xpos -981
      ypos -1509
     }
     Multiply {
      channels rgb
      value {{parent.darkvalues}}
      name Multiply1
      xpos -981
      ypos -1457
     }
     Add {
      channels rgba
      value 1
      name Add4
      xpos -981
      ypos -1400
     }
     Clamp {
      channels rgb
      name Clamp3
      xpos -981
      ypos -1326
     }
     Dot {
      name Dot21
      xpos -950
      ypos -1166
     }
    push $Nc514c00
     Merge2 {
      inputs 2+1
      operation multiply
      bbox B
      maskChannelMask -rgba.alpha
      name Merge6
      xpos 559
      ypos -1166
      disable {{parent.disabledarkvalues}}
     }
     Merge2 {
      inputs 2+1
      operation plus
      maskChannelMask -rgba.alpha
      name Merge7
      xpos 559
      ypos -231
      disable {{parent.disablebrightvalues}}
     }
    push $Nc515400
     Merge2 {
      inputs 2
      operation copy
      mix {{parent.mix}}
      name Merge1
      xpos 559
      ypos 65
     }
     Copy {
      inputs 2
      from0 rgba.alpha
      to0 rgba.alpha
      name Copy1
      xpos 559
      ypos 355
     }
     Log2Lin {
      name Log2Lin2
      xpos 559
      ypos 1101
      disable {{"1 - parent.logspace"}}
     }
     Dot {
      name Dot28
      xpos 590
      ypos 1175
     }
     Output {
      name Output1
      xpos 559
      ypos 1249
     }
     StickyNote {
      inputs 0
      name StickyNote2
      label "ADDING FG BRIGHT VALUES"
      note_font "Verdana Bold"
      note_font_size 25
      xpos 636
      ypos -312
     }
     StickyNote {
      inputs 0
      name StickyNote1
      label "ADDING FG DARK VALUES"
      note_font "Verdana Bold"
      note_font_size 25
      xpos 647
      ypos -1274
     }
    push $Nc3c7800
    push $Nc3c6800
     Viewer {
      inputs 2
      frame_range 1-262
      input_number 1
      colour_sample_bbox {0.076171875 0.2724609375 0.0771484375 0.2734375}
      viewerProcess "sRGB (ACES)"
      name Viewer1
      xpos 593
      ypos 1397
     }
    end_group
```
