---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/lens-artefact/","tags":["nuke","node","lens","aberration"]}
---


# Lens Artefact

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
     name LensArtefact
     tile_color 0xdb7855ff
     label "\n"
     note_font "DejaVu Sans"
     selected true
     xpos -6504
     ypos -720
     addUserKnob {20 LensArtefact l Lens_Artefact}
     addUserKnob {26 lens_distortion l "LENS DISTORTION"}
     addUserKnob {41 distortion1 l "Radial Distortion 1" T LensDistortion2.distortion1}
     addUserKnob {41 distortion2 l "Radial Distortion 2" T LensDistortion2.distortion2}
     addUserKnob {26 ""}
     addUserKnob {26 chromatic_aberration l "CHROMATIC ABERRATION"}
     addUserKnob {41 IN l "IN channels" t "The colour that leaks inward." T GodRays_OUT.channels}
     addUserKnob {41 OUT l "OUT channels" t "The colour that leaks outward.\n" T GodRays_IN.channels}
     addUserKnob {6 which_1_1 l "Light Lens Blur" t which_1 +STARTLINE}
     addUserKnob {26 SCALE}
     addUserKnob {41 scale l "IN scale" t "How much the <b>IN</b> leaks inward." T GodRays_OUT.scale}
     addUserKnob {41 scale_1 l "OUT scale" t "How much the <b> OUT </b> leaks outward." T GodRays_IN.scale}
     addUserKnob {41 size l "FRINGE size" T Blur3.size}
     addUserKnob {26 ""}
     addUserKnob {41 which l Mix T Dissolve1.which}
    }
     Input {
      inputs 0
      name mask
      xpos -396
      ypos 627
      number 1
     }
     Dot {
      name Dot3
      xpos -362
      ypos 671
     }
    set N2be32270 [stack 0]
     Dot {
      name Dot2
      xpos -362
      ypos 713
     }
    set N2be32710 [stack 0]
     Dot {
      name Dot1
      xpos -362
      ypos 752
     }
    set N331aec00 [stack 0]
     Dot {
      name Dot8
      xpos -362
      ypos 820
     }
    push $N331aec00
    push $N2be32710
    push $N2be32270
     Input {
      inputs 0
      name Input1
      xpos -588
      ypos 390
     }
     Dot {
      name Dot9
      xpos -554
      ypos 431
     }
    set N2be30480 [stack 0]
     LensDistortion {
      serializeKnob ""
      serialiseKnob "22 serialization::archive 17 0 0 0 0 0 0 0 0 0 0 0 0"
      anamorphicSqueeze 0.465
      name LensDistortion2
      xpos -588
      ypos 487
     }
     Transform {
      scale {{parent.LensDistortion2.distortion1<0?LensDistortion2.distortion1*-1+1.006:1 x1321 0.2 x1335 0.2}}
      center {{input.format.w/2} {input.format.h/2}}
      filter Mitchell
      shutteroffset centred
      name Transform1
      xpos -588
      ypos 513
     }
     Transform {
      scale {{parent.LensDistortion2.distortion2<0?LensDistortion2.distortion2*-1+1.006:1}}
      center {{input.format.w/2} {input.format.h/2}}
      filter Mitchell
      shutteroffset centred
      name Transform2
      xpos -588
      ypos 544
     }
     Dot {
      name Dot5
      xpos -554
      ypos 603
     }
    set N27c1c0e0 [stack 0]
     Dot {
      name Dot4
      xpos -457
      ypos 603
     }
     GodRays {
      inputs 1+1
      channels rgba
      scale 1.002
      center {{input.format.w/2} {input.format.h/2}}
      name GodRays_LENSBLUR
      xpos -491
      ypos 667
     }
    push $N27c1c0e0
     Switch {
      inputs 2
      which {{"\[value which_1_1]" x1 1}}
      name Switch1
      label "\[value which]"
      xpos -588
      ypos 661
     }
     GodRays {
      inputs 1+1
      channels {-rgba.red -rgba.green rgba.blue none}
      scale 0.999
      center {{input.format.w/2} {input.format.h/2}}
      name GodRays_IN
      xpos -588
      ypos 709
     }
     GodRays {
      inputs 1+1
      channels {rgba.red -rgba.green -rgba.blue none}
      scale 1.001
      center {{input.format.w/2} {input.format.h/2}}
      name GodRays_OUT
      xpos -588
      ypos 748
     }
     Blur {
      inputs 1+1
      channels {-rgba.red -rgba.green rgba.blue none}
      size 3
      name Blur3
      label "\[value size]"
      xpos -588
      ypos 810
     }
    push $N2be30480
     Dot {
      name Dot7
      xpos -651
      ypos 431
     }
     Dot {
      name Dot6
      xpos -651
      ypos 964
     }
     Dissolve {
      inputs 2
      which 1
      name Dissolve1
      label "Global Mix\n"
      xpos -588
      ypos 948
     }
     Output {
      name Output1
      xpos -588
      ypos 1009
     }
    end_group
```
