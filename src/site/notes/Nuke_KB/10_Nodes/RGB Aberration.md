---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/rgb-aberration/","tags":["nuke","node","aberration","lens","color"]}
---


# RGB Aberration

## 🧠 Description


## 🎯 When to use


## ⚠️ Known pitfalls


## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```nuke
    set cut_paste_input [stack 0]
    version 13.2 v8
    BackdropNode {
     inputs 0
     name BKD_RGB_Abberation1
     tile_color 0x9284e0ff
     label "RGB ABERRATION"
     note_font "Arial Bold"
     note_font_size 100
     selected true
     xpos -9952
     ypos -1114
     bdwidth 1601
     bdheight 565
     z_order -1
    }
    push $cut_paste_input
    Dot {
     name Dot190
     label " COMP"
     note_font "Bitstream Vera Sans Bold"
     note_font_size 18
     selected true
     xpos -9318
     ypos -897
    }
    Dot {
     name Dot134
     selected true
     xpos -9318
     ypos -807
    }
    Dot {
     inputs 0
     name Dot192
     label " COMP ALPHA"
     note_font "Bitstream Vera Sans Bold"
     note_font_size 18
     selected true
     xpos -9139
     ypos -897
    }
    Grade {
     channels alpha
     gamma 2
     white_clamp true
     name Thicken_Alpha1
     selected true
     xpos -9173
     ypos -787
    }
    Dot {
     name Dot193
     selected true
     xpos -9142
     ypos -714
    }
    Dot {
     inputs 0
     name Link_DGN_COMP1
     tile_color 0xff9100ff
     label "  Connected to :\n  \[regsub Anchor_ \[value Tlink] \"\"]\n\n"
     note_font_size 20
     note_font_color 0xb26300ff
     selected true
     xpos -9620
     ypos -714
     hide_input true
     addUserKnob {20 Options}
     addUserKnob {1 Tlink l "" +STARTLINE +HIDDEN}
     Tlink Anchor_DGN_COMP
     addUserKnob {22 duplicate l "Duplicate Link" -STARTLINE T anchorLink.LinkDot.duplicateLink(nuke.thisNode())}
     addUserKnob {22 connect l "Reconnect Selection" -STARTLINE T anchorLink.LinkDot.connect(nuke.selectedNodes())}
     addUserKnob {26 "Family OP"}
     addUserKnob {22 rename l Rename -STARTLINE T anchorLink.AnchorDot.renameByFamily(nuke.thisNode())}
     addUserKnob {22 select l Select -STARTLINE T anchorLink.AnchorDot.selectByFamily(nuke.thisNode())}
     addUserKnob {22 recolor l "Recolor from parent" -STARTLINE T anchorLink.AnchorDot.changeColorByFamily(nuke.thisNode())}
     addUserKnob {26 "Graph UI"}
     addUserKnob {22 showHide l "Show / hide inputs ALL" -STARTLINE T anchorLink.LinkDot.toggleInputs(nuke.allNodes('Dot'))}
     addUserKnob {26 "Navigate to"}
     addUserKnob {22 navParent l Anchor -STARTLINE T anchorLink.LinkDot.navToParent(nuke.thisNode())}
     addUserKnob {22 navCurrent l Link -STARTLINE T anchorLink.LinkDot.navToCurrent(nuke.thisNode())}
    }
    Group {
     inputs 2
     name RGB_Abberation1
     tile_color 0xffff
     gl_color 0xffffffff
     label "\n\[value output_mode]\n\[if \{\[value visu_edges]==\"false\" && \[value visu_channels]==\"false\"\} \{if \{\[value Output_mode.which]==\"0\"\} \{return \"\[knob this.tile_color 0x00ff00ff]\"\} \{return \"\[knob this.tile_color 0x0000ffff]\"\}\} \{return \"VISUALIZATION MODE \[knob this.tile_color 0xff0000ff]\"\}]\nFilter: \[value filter] - Log to Lin: \[value logtolin_check]\n-"
     note_font "Bitstream Vera Sans Bold"
     note_font_color 0xff
     selected true
     xpos -9346
     ypos -745
     addUserKnob {20 rgb_abberation l "RGB abberation"}
     addUserKnob {4 output_mode l "Output Mode" M {"REMOVE abberation" "APPLY abberation" "" "" "" ""}}
     addUserKnob {26 ""}
     addUserKnob {6 logtolin_check l "log to lin" +STARTLINE}
     addUserKnob {41 in_colorspace l in -STARTLINE T OCIOColorSpace_IN.in_colorspace}
     addUserKnob {41 out_colorspace l out -STARTLINE T OCIOColorSpace_IN.out_colorspace}
     addUserKnob {26 ""}
     addUserKnob {20 visualization l "Visualization Help" n 1}
     addUserKnob {6 visu_edges l "Edges detect" t "Activates an edge treartment to better estimate original abberation" +STARTLINE}
     addUserKnob {41 blursize l "blur size" -STARTLINE T EdgeDetect_R.blursize}
     addUserKnob {26 ""}
     addUserKnob {6 visu_channels l "Split channels" +STARTLINE}
     addUserKnob {41 turn l vertical -STARTLINE T Reformat1.turn}
     addUserKnob {7 stripes R 1 50}
     stripes 24
     addUserKnob {20 endGroup n -1}
     addUserKnob {26 ""}
     addUserKnob {20 abbparameters l "Abberation parameters (size in pixels)" n 1}
     addUserKnob {6 abb_by_transform l "Zoom Abberation" +STARTLINE}
     abb_by_transform true
     addUserKnob {41 filter -STARTLINE T NEUTRAL.filter}
     addUserKnob {7 sizeR l "size R" R 0 10}
     sizeR 1.2
     addUserKnob {7 sizeG l "size G" R 0 10}
     addUserKnob {7 sizeB l "size B" R 0 10}
     sizeB 1.8
     addUserKnob {26 ""}
     addUserKnob {6 abb_by_blur l "Blur Abberation" +STARTLINE}
     addUserKnob {41 blurRsize l "blur R" T Blur_R.size}
     addUserKnob {41 blurGsize l "blur G" T Blur_G.size}
     addUserKnob {41 blurBsize l "blur B" T Blur_B.size}
     addUserKnob {26 ""}
     addUserKnob {6 abb_A l "apply abberation to alpha: " +STARTLINE}
     abb_A true
     addUserKnob {41 mix l "" -STARTLINE T Copy_A.mix}
     addUserKnob {20 endGroup_1 l endGroup n -1}
     addUserKnob {26 ""}
     addUserKnob {26 signature l "" +STARTLINE T philippe.desfretier@technicolor.com}
     addUserKnob {20 help_1 l help}
     addUserKnob {26 text l "" +STARTLINE T "Assemble a \"comp alpha\" of all comp-modified areas\nneeding plate-matching abberation.\n\nPlug to DGN, switch to \"REMOVE Abberation\" mode,\nand adjust zoom and/or blur parameters to remove abberation.\n\nTry out \"log to lin\" parameters for better results.\n\nTry out visualization help (edge detect\nand channel stripes) to better adjust parameters.\n\nWhen abberation seems removed well enough from the plate,\nand switch to \"APPLY abberation\" mode,\nand plug to Comp and Comp Alpha\n\nAdjust \"Thicken alpha\" parameters to adjust\nhow much abberation applies to comp edges."}
    }
     BackdropNode {
      inputs 0
      name BackdropNode1
      tile_color 0x6d00ff
      label "APPLY Abberation"
      note_font "Bitstream Vera Sans Bold"
      note_font_size 42
      xpos -317
      ypos 264
      appearance Border
      border_width 20
      bdwidth 955
      bdheight 1031
     }
     BackdropNode {
      inputs 0
      name BackdropNode2
      tile_color 0x2e7fff
      label "REMOVE Abberation"
      note_font "Bitstream Vera Sans Bold"
      note_font_size 42
      xpos -1597
      ypos 264
      appearance Border
      border_width 20
      bdwidth 955
      bdheight 1031
     }
     BackdropNode {
      inputs 0
      name BackdropNode3
      tile_color 0x7f0000ff
      label "VISU EDGES"
      note_font "Bitstream Vera Sans Bold"
      note_font_size 42
      xpos -188
      ypos -819
      appearance Border
      border_width 20
      bdwidth 857
      bdheight 537
     }
     BackdropNode {
      inputs 0
      name BackdropNode4
      tile_color 0x7f0000ff
      label "VISU CHANNELS"
      note_font "Bitstream Vera Sans Bold"
      note_font_size 42
      xpos 277
      ypos 2067
      appearance Border
      border_width 20
      bdwidth 830
      bdheight 726
     }
     Reformat {
      inputs 0
      type "to box"
      box_width 1
      box_height 3
      box_fixed true
      name Reformat3
      label "Resize: \[value resize] - center: \[value center]\nFilter: \[value filter]"
      xpos 840
      ypos 2198
     }
     Shuffle2 {
      fromInput1 {{0} B}
      fromInput2 {{0} B}
      mappings "4 white -1 -1 rgba.red 0 0 white -1 -1 rgba.green 0 1 white -1 -1 rgba.blue 0 2 white -1 -1 rgba.alpha 0 3"
      name Shuffle15
      label "\[if \{ \[value in1] == \[value out1]\} \{ return \"\[string toupper \[value in1]] \" \} else \{ return \"\[string toupper \[value in1]]-->\[string toupper \[value out1]]\" \} ]"
      xpos 840
      ypos 2260
     }
     Dot {
      name Dot32
      xpos 874
      ypos 2304
     }
    set Nea18d170 [stack 0]
     Dot {
      name Dot26
      xpos 954
      ypos 2304
     }
    set Nea1920a0 [stack 0]
     Dot {
      name Dot25
      xpos 1034
      ypos 2304
     }
     Crop {
      box {0 0 1 1}
      name Crop4
      xpos 1000
      ypos 2330
     }
     Dot {
      name Dot27
      xpos 1034
      ypos 2394
     }
    push $Nea1920a0
     Crop {
      box {0 1 1 2}
      name Crop5
      xpos 920
      ypos 2330
     }
     Dot {
      name Dot28
      xpos 954
      ypos 2364
     }
    push $Nea18d170
     Crop {
      box {0 2 1 3}
      name Crop6
      xpos 840
      ypos 2330
     }
     Copy {
      inputs 2
      from0 rgba.green
      to0 rgba.green
      name Copy3
      xpos 840
      ypos 2354
     }
     Copy {
      inputs 2
      from0 rgba.blue
      to0 rgba.blue
      name Copy4
      xpos 840
      ypos 2384
     }
     Crop {
      box {0 0 1 3}
      crop false
      name Crop7
      xpos 840
      ypos 2428
     }
     Reformat {
      type "to box"
      box_width {{parent.DETECT_FORMAT.box.r}}
      box_height {{parent.DETECT_FORMAT.box.t}}
      box_fixed true
      resize distort
      filter impulse
      name Reformat1
      label "Resize: \[value resize] - center: \[value center]\nFilter: \[value filter]"
      xpos 840
      ypos 2498
     }
     Tile {
      rows {{parent.stripes/3}}
      columns {{parent.stripes/3}}
      filter impulse
      name Tile1
      xpos 840
      ypos 2570
     }
     Shuffle {
      alpha white
      name Shuffle12
      label "\[value in] -> \[value out]\nA = 1"
      xpos 840
      ypos 2618
     }
     Dot {
      name Dot24
      xpos 874
      ypos 2724
     }
     Input {
      inputs 0
      name InputImg
      note_font "DejaVu Sans Bold"
      note_font_size 24
      xpos 440
      ypos -1077
     }
     Dot {
      name Dot29
      xpos 474
      ypos -916
     }
    set Nea222030 [stack 0]
     Dot {
      name Dot15
      xpos 474
      ypos -706
     }
    set Nea226e70 [stack 0]
     Dot {
      name Dot16
      xpos 314
      ypos -706
     }
    set Nea22bd80 [stack 0]
     Shuffle {
      red black
      green black
      alpha black
      name Shuffle8
      label "\[value in] -> \[value out]\nB"
      xpos 280
      ypos -682
     }
     EdgeDetectWrapper {
      threshold {{parent.EdgeDetect_R.threshold}}
      output rgba.blue
      erodesize {{parent.EdgeDetect_R.erodesize}}
      blursize {{parent.EdgeDetect_R.blursize}}
      blurquality {{parent.EdgeDetect_R.blurquality}}
      name EdgeDetect_B
      xpos 280
      ypos -626
     }
     Dot {
      name Dot17
      xpos 314
      ypos -556
     }
    push 0
    push $Nea22bd80
     Dot {
      name Dot18
      xpos 154
      ypos -706
     }
    set Nea257d30 [stack 0]
     Shuffle {
      red black
      blue black
      alpha black
      name Shuffle7
      label "\[value in] -> \[value out]\nG"
      xpos 120
      ypos -682
     }
     EdgeDetectWrapper {
      threshold {{parent.EdgeDetect_R.threshold}}
      output rgba.green
      erodesize {{parent.EdgeDetect_R.erodesize}}
      blursize {{parent.EdgeDetect_R.blursize}}
      blurquality {{parent.EdgeDetect_R.blurquality}}
      name EdgeDetect_G
      xpos 120
      ypos -626
     }
     Dot {
      name Dot19
      xpos 154
      ypos -556
     }
    push $Nea257d30
     Dot {
      name Dot20
      xpos -6
      ypos -706
     }
     Shuffle {
      green black
      blue black
      alpha black
      name Shuffle6
      label "\[value in] -> \[value out]\nR"
      xpos -40
      ypos -682
     }
     EdgeDetectWrapper {
      output rgba.red
      name EdgeDetect_R
      xpos -40
      ypos -626
     }
     Dot {
      name Dot22
      xpos -6
      ypos -554
     }
     Merge2 {
      inputs 3+1
      operation plus
      name Merge2
      xpos -40
      ypos -500
     }
     Dot {
      name Dot21
      xpos -6
      ypos -406
     }
    push $Nea226e70
     Copy {
      inputs 2
      from0 -rgba.alpha
      to0 -rgba.alpha
      channels rgb
      name Copy_RGB1
      xpos 440
      ypos -422
      disable {{!parent.visu_edges}}
     }
     OCIOColorSpace {
      in_colorspace linear
      out_colorspace compositing_log
      name OCIOColorSpace_IN
      note_font "Bitstream Vera Sans Bold"
      xpos 440
      ypos -70
      disable {{!parent.logtolin_check}}
     }
     Dot {
      name Dot145
      xpos 474
      ypos 144
     }
    set Nea2e22d0 [stack 0]
     Dot {
      name Dot13
      xpos -1286
      ypos 144
     }
     Dot {
      name Dot10
      xpos -1286
      ypos 474
     }
    set Nea2ebe90 [stack 0]
     Dot {
      name Dot11
      xpos -1126
      ypos 474
     }
    set Nea2f0dc0 [stack 0]
     Dot {
      name Dot12
      xpos -966
      ypos 474
     }
     Shuffle {
      red black
      green black
      name Shuffle2
      label "\[value in] -> \[value out]\nB & A"
      xpos -1000
      ypos 518
     }
     Transform {
      scale {{(input.width+parent.sizeB)/input.width}}
      center {{input.width/2} {input.height/2}}
      invert_matrix true
      black_outside false
      shutteroffset centred
      name Transform_B1
      xpos -1000
      ypos 590
      disable {{1-parent.abb_by_transform}}
     }
     Reformat {
      type scale
      resize none
      center false
      pbb true
      name NEUTRAL
      label "Resize: \[value resize] - center: \[value center]\nFilter: \[value filter]\nFOR FILTER SELECTION"
      xpos -1000
      ypos 662
     }
     set Cea31a860 [stack 0]
     Sharpen {
      channels {-rgba.red -rgba.green rgba.blue none}
      size {{parent.Blur_B.size}}
      name Sharpen_B
      xpos -1000
      ypos 770
      disable {{!parent.abb_by_blur}}
     }
     Dot {
      name Dot93
      xpos -966
      ypos 894
     }
    push 0
    push $Nea2f0dc0
     Shuffle {
      red black
      blue black
      name Shuffle1
      label "\[value in] -> \[value out]\nG & A"
      xpos -1160
      ypos 518
     }
     Transform {
      scale {{(input.width+parent.sizeG)/input.width}}
      center {{input.width/2} {input.height/2}}
      invert_matrix true
      black_outside false
      shutteroffset centred
      name Transform_G1
      xpos -1160
      ypos 590
      disable {{1-parent.abb_by_transform}}
     }
    clone $Cea31a860 {
      xpos -1160
      ypos 662
      selected false
     }
     Sharpen {
      channels {-rgba.red rgba.green -rgba.blue none}
      size {{parent.Blur_G.size}}
      name Sharpen_G
      xpos -1160
      ypos 770
      disable {{!parent.abb_by_blur}}
     }
     Dot {
      name Dot92
      xpos -1126
      ypos 894
     }
    push $Nea2ebe90
     Shuffle {
      green black
      blue black
      name Shuffle13
      label "\[value in] -> \[value out]\nR & A"
      xpos -1320
      ypos 518
     }
     Transform {
      scale {{(input.width+parent.sizeR)/input.width}}
      center {{input.width/2} {input.height/2}}
      invert_matrix true
      black_outside false
      shutteroffset centred
      name Transform_R1
      xpos -1320
      ypos 590
      disable {{1-parent.abb_by_transform}}
     }
    clone $Cea31a860 {
      xpos -1320
      ypos 662
      selected false
     }
     Sharpen {
      channels {rgba.red -rgba.green -rgba.blue -rgba.alpha}
      size {{parent.Blur_R.size}}
      name Sharpen_R
      xpos -1320
      ypos 770
      disable {{!parent.abb_by_blur}}
     }
     Dot {
      name Dot94
      xpos -1286
      ypos 894
     }
     Merge2 {
      inputs 3+1
      operation plus
      name Merge16
      xpos -1320
      ypos 980
     }
     Clamp {
      channels rgb
      maximum_enable false
      name Clamp1
      xpos -1320
      ypos 1100
      disable {{!parent.abb_by_blur}}
     }
     Dot {
      name Dot14
      xpos -1286
      ypos 1614
     }
     Input {
      inputs 0
      name Inputmask
      xpos 1560
      ypos 1280
      number 1
     }
     Remove {
      operation keep
      channels alpha
      name keep_A1
      xpos 1560
      ypos 1334
     }
     Invert {
      channels alpha
      name Invert1
      xpos 1560
      ypos 1424
     }
     Dot {
      name Dot5
      xpos 1594
      ypos 1494
     }
    push $Nea2e22d0
     Dot {
      name Dot23
      xpos 1274
      ypos 144
     }
     Dot {
      name Dot4
      xpos 1274
      ypos 1344
     }
    push $Nea2e22d0
     Dot {
      name Dot2
      xpos 474
      ypos 384
     }
    set Nea412170 [stack 0]
     Dot {
      name Dot9
      xpos -166
      ypos 384
     }
     Dot {
      name Dot135
      xpos -166
      ypos 474
     }
    set Nea41bfd0 [stack 0]
     Dot {
      name Dot136
      xpos -6
      ypos 474
     }
    set Nea420f00 [stack 0]
     Dot {
      name Dot137
      xpos 154
      ypos 474
     }
     Shuffle {
      red black
      green black
      name Shuffle5
      label "\[value in] -> \[value out]\nB & A"
      xpos 120
      ypos 518
     }
     Transform {
      scale {{(input.width+parent.sizeB)/input.width}}
      center {{input.width/2} {input.height/2}}
      black_outside false
      shutteroffset centred
      name Transform_B
      xpos 120
      ypos 590
      disable {{1-parent.abb_by_transform}}
     }
    clone $Cea31a860 {
      xpos 120
      ypos 662
      selected false
     }
     Blur {
      channels {-rgba.red -rgba.green rgba.blue rgba.alpha}
      crop false
      name Blur_B
      xpos 120
      ypos 764
      disable {{!parent.abb_by_blur}}
     }
     Dot {
      name Dot8
      xpos 154
      ypos 894
     }
    set Nea468240 [stack 0]
    push 0
    push $Nea420f00
     Shuffle {
      red black
      blue black
      name Shuffle4
      label "\[value in] -> \[value out]\nG & A"
      xpos -40
      ypos 518
     }
     Transform {
      scale {{(input.width+parent.sizeG)/input.width}}
      center {{input.width/2} {input.height/2}}
      black_outside false
      shutteroffset centred
      name Transform_G
      xpos -40
      ypos 590
      disable {{1-parent.abb_by_transform}}
     }
    clone $Cea31a860 {
      xpos -40
      ypos 662
      selected false
     }
     Blur {
      channels {-rgba.red rgba.green -rgba.blue rgba.alpha}
      name Blur_G
      xpos -40
      ypos 764
      disable {{!parent.abb_by_blur}}
     }
     Dot {
      name Dot7
      xpos -6
      ypos 894
     }
    set Nea4aa610 [stack 0]
    push $Nea41bfd0
     Shuffle {
      green black
      blue black
      name Shuffle3
      label "\[value in] -> \[value out]\nR & A"
      xpos -200
      ypos 518
     }
     Transform {
      scale {{(input.width+parent.sizeR)/input.width}}
      center {{input.width/2} {input.height/2}}
      black_outside false
      shutteroffset centred
      name Transform_R
      xpos -200
      ypos 590
      disable {{1-parent.abb_by_transform}}
     }
    clone $Cea31a860 {
      xpos -200
      ypos 662
      selected false
     }
     Blur {
      channels {rgba.red -rgba.green -rgba.blue rgba.alpha}
      name Blur_R
      xpos -200
      ypos 764
      disable {{!parent.abb_by_blur}}
     }
     Dot {
      name Dot6
      xpos -166
      ypos 894
     }
    set Nea4ec9e0 [stack 0]
     Merge2 {
      inputs 3+1
      operation average
      name Merge1
      xpos -200
      ypos 980
     }
     Remove {
      operation keep
      channels alpha
      name keep_A
      xpos -200
      ypos 1034
     }
     Dot {
      name Dot1
      xpos -166
      ypos 1224
     }
    push $Nea468240
    push 0
    push $Nea4aa610
    push $Nea4ec9e0
     Merge2 {
      inputs 3+1
      operation plus
      name Merge43
      xpos 120
      ypos 978
     }
     Remove {
      operation keep
      channels rgb
      name keep_RGB
      xpos 120
      ypos 1040
     }
     Dot {
      name Dot144
      xpos 154
      ypos 1134
     }
    push $Nea412170
     Copy {
      inputs 2
      from0 -rgba.alpha
      to0 -rgba.alpha
      channels rgb
      name Copy_RGB
      xpos 440
      ypos 1118
     }
     ChannelMerge {
      inputs 2
      operation "b if not a"
      name Copy_A
      label "\[value mix]"
      xpos 440
      ypos 1202
      disable {{!parent.abb_A}}
     }
     Keymix {
      inputs 3
      name Keymix1
      xpos 440
      ypos 1484
     }
     Switch {
      inputs 2
      which {{!parent.output_mode}}
      name Output_mode
      label "\[value which]"
      xpos 440
      ypos 1604
     }
     OCIOColorSpace {
      in_colorspace {{OCIOColorSpace_IN.out_colorspace}}
      out_colorspace {{OCIOColorSpace_IN.in_colorspace}}
      name OCIOColorSpace_OUT
      note_font "Bitstream Vera Sans Bold"
      xpos 440
      ypos 1850
      disable {{!parent.logtolin_check x3 1}}
     }
     Merge2 {
      inputs 2
      operation multiply
      bbox B
      Achannels rgb
      Bchannels rgb
      output rgb
      name Merge4
      xpos 440
      ypos 2720
      disable {{!parent.visu_channels}}
     }
     Dot {
      name Dot134
      xpos 474
      ypos 2994
     }
     Output {
      name Output
      note_font "Bitstream Vera Sans Bold"
      note_font_size 24
      xpos 440
      ypos 3095
     }
    push $Nea222030
     Crop {
      box {{input.format.x} {input.format.y} {input.format.r} {input.format.t}}
      name DETECT_FORMAT
      xpos 760
      ypos -920
     }
    end_group
    Dot {
     name Dot194
     selected true
     xpos -9318
     ypos -627
    }
    StickyNote {
     inputs 0
     name StickyNote19
     tile_color 0x5e5491ff
     label "<align=left><p style=\"font-family:arial\">\n\nAssemble a \"comp alpha\" of all comp-modified areas\nneeding plate-matching abberation.\n\nPlug to DGN, switch to \"REMOVE Abberation\" mode,\nand adjust zoom and/or blur parameters to remove abberation.\n\nTry out \"log to lin\" parameters for better results.\n\nTry out visualization help (edge detect\nand channel stripes) to better adjust parameters.\n\nWhen abberation seems removed well enough from the plate,\nand switch to \"APPLY abberation\" mode,\nand plug to Comp and Comp Alpha\n\nAdjust \"Thicken alpha\" parameters to adjust\nhow much abberation applies to comp edges.\n\n"
     note_font_size 14
     selected true
     xpos -8968
     ypos -949
    }
```
