---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/project3-d-setup/","tags":["nuke","node","rendering","scanline","3D"]}
---


# Project3DSetup

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
    name Project3DSetup3
    tile_color 0x9c000000
    label "\[if \{\[value stabilize] == \"true\" \} \{return \"Stabilizing\"\} \{return \"Projecting\"\}]\nref frame = \[value reference_frame]\n\n\n\[if \{\[value stabilize]== \"true\" \} \{return \"\[knob this.tile_color 0xc64545ff]\"\} \{return \"\[knob this.tile_color 0x9c000000]\"\}]"
    selected true
    xpos 2542
    ypos 8900
    addUserKnob {20 User}
    addUserKnob {26 ""}
    addUserKnob {41 stabilize l Stabilize T Stabilize.stabilize}
    addUserKnob {41 fillMatEnabled l "Enable FillMat" T FIllMatControl.fillMatEnabled}
    addUserKnob {26 ""}
    addUserKnob {3 reference_frame l "reference frame"}
    reference_frame 1183
    addUserKnob {22 set_to_current_frame l "Set to current frame" -STARTLINE T "nuke.thisNode().knob(\"reference_frame\").setValue(nuke.frame())"}
    addUserKnob {26 ""}
    addUserKnob {20 Project3D}
    addUserKnob {41 project_on l "project on" T Project3D29.project_on}
    addUserKnob {41 crop -STARTLINE T Project3D29.crop}
    addUserKnob {41 occlusion_mode l "occlusion mode" T Project3D29.occlusion_mode}
    addUserKnob {20 ScanlineRender}
    addUserKnob {41 filter T ScanlineRender31.filter}
    addUserKnob {41 antialiasing T ScanlineRender31.antialiasing}
    addUserKnob {41 overscan T ScanlineRender31.overscan}
    addUserKnob {41 samples T ScanlineRender31.samples}
    addUserKnob {41 shutter T ScanlineRender31.shutter}
    addUserKnob {41 shutteroffset l "shutter offset" T ScanlineRender31.shutteroffset}
    }
    Input {
    inputs 0
    name camera
    xpos -490
    ypos 381
    number 2
    }
    Dot {
    name Dot118
    xpos -462
    ypos 438
    }
    set N2921bdb0 [stack 0]
    FrameHold {
    firstFrame {{parent.reference_frame}}
    name FrameHold1
    xpos -490
    ypos 819
    disable {{"parent.Stabilize.stabilize == 1 ? 0 : 1"}}
    addUserKnob {20 User}
    addUserKnob {22 setToCurrentFrameButton l "Set to current frame" -STARTLINE T vfxNukeScripts.python.miFrameHold._on_set_current_frame_button_clicked()}
    }
    push $N2921bdb0
    FrameHold {
    firstFrame {{parent.reference_frame x1040 1040}}
    name FrameHold62
    xpos -653
    ypos 434
    disable {{parent.Stabilize.stabilize}}
    addUserKnob {20 User}
    addUserKnob {22 setToCurrentFrameButton l "Set to current frame" -STARTLINE T vfxNukeScripts.python.miFrameHold._on_set_current_frame_button_clicked()}
    }
    Input {
    inputs 0
    name img
    xpos -803
    ypos 367
    }
    Project3D2 {
    inputs 2
    name Project3D29
    xpos -803
    ypos 440
    }
    Dot {
    name Dot1
    xpos -775
    ypos 532
    }
    set N6795c7c0 [stack 0]
    push $N6795c7c0
    Dot {
    name Dot2
    xpos -692
    ypos 532
    }
    FillMat {
    inputs 0
    name FillMat1
    xpos -588
    ypos 584
    }
    MergeMat {
    inputs 2
    also_merge all
    name MergeMat1
    xpos -720
    ypos 584
    }
    Dot {
    name Dot3
    xpos -692
    ypos 618
    }
    Switch {
    inputs 2
    name Switch1
    xpos -803
    ypos 620
    }
    set Nc611a30 [stack 0]
    Dot {
    name Dot119
    xpos -775
    ypos 674
    }
    Input {
    inputs 0
    name geo
    xpos -952
    ypos 440
    number 1
    }
    Dot {
    name Dot120
    xpos -924
    ypos 593
    }
    ApplyMaterial {
    inputs 2
    name ApplyMaterial3
    selected true
    xpos -952
    ypos 676
    }
    Dot {
    name Dot121
    xpos -924
    ypos 823
    }
    Input {
    inputs 0
    name bg
    label bg
    xpos -810
    ypos 728
    number 3
    }
    ScanlineRender {
    inputs 3
    conservative_shader_sampling false
    samples 15
    shutteroffset centred
    motion_vectors_type distance
    name ScanlineRender31
    xpos -810
    ypos 825
    }
    Output {
    name Output1
    xpos -810
    ypos 926
    }
    push $Nc611a30
    push 0
    Viewer {
    inputs 2
    frame_range 1001-1190
    input_number 1
    name Viewer1
    xpos -660
    ypos 825
    }
    NoOp {
    inputs 0
    name FIllMatControl
    knobChanged "n =nuke.thisNode()\nk = nuke.thisKnob()\n\nswitch = nuke.toNode(\"Switch1\")\n\n\n\nif k.name() == \"fillMatEnabled\":\n    if k.value():\n        switch\[\"which\"].setValue(0)\n    else:\n        switch\[\"which\"].setValue(1)"
    xpos -362
    ypos 800
    addUserKnob {20 User}
    addUserKnob {6 fillMatEnabled l "Enable FillMat" +STARTLINE}
    fillMatEnabled true
    }
    NoOp {
    inputs 0
    name bg_ctrl
    xpos -340
    ypos 664
    addUserKnob {20 User}
    addUserKnob {6 bg_switch l "custom bg" +STARTLINE}
    }
    NoOp {
    inputs 0
    name Stabilize
    xpos -358
    ypos 570
    addUserKnob {20 User}
    addUserKnob {6 stabilize l Stabilize t "Stabilized - if checked\nProject - if unchecked" +STARTLINE}
    }
    end_group
```
