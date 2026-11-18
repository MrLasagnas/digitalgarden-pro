---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/image-plane/","tags":["nuke","node","3D","tracking"]}
---



## 🧠 Description

Put your input on a 3D Card at a certain distance from the camera.
## 🎯 When to use

Helpful to quickly track a roto or a patch into your shot using the shot camera.
## ⚠️ Known pitfalls

## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```
set cut_paste_input [stack 0]
version 13.0 v10
push $cut_paste_input
Group {
 name ImagePlane
 knobChanged "def add_input(group, prefix):\n    with group:\n        if nuke.toNode(prefix) is None:\n            input = nuke.nodes.Input()\n            input.setName(prefix)\n            noop_input = nuke.toNode(prefix+'_input')\n            noop_input.setInput(0, input)\n            xpos = noop_input.xpos()\n            ypos = noop_input.ypos()\n            input.setXYpos(xpos, ypos-50)\n\ndef remove_input(group, prefix):\n    with group:\n        nuke.delete(nuke.toNode(prefix))\n        \ngroup = nuke.thisNode()\nknobName = nuke.thisKnob().name()\n\nif knobName == 'use_ref_cam':\n    if group\['use_ref_cam'].getValue() == True:\n        add_input(group, 'ref_cam')\n    else:\n        remove_input(group, 'ref_cam')"
 selected true
 xpos -17
 ypos 3
 addUserKnob {20 ImagePlane}
 addUserKnob {26 title l "" +STARTLINE T "<b>ImagePlane v1.0</b>"}
 addUserKnob {26 author l "" +STARTLINE T "<font color=\"#888\" size=\"2\">by Nikolai Wüstemann</font>"}
 addUserKnob {26 ""}
 addUserKnob {7 distance t "distance from cam" R 0.0001 100}
 distance 10
 addUserKnob {3 ref_frame l "reference frame"}
 ref_frame 1001
 addUserKnob {6 use_ref_cam l "use ref cam" t "Can be useful, when copied from another shot with another cam." -STARTLINE}
 addUserKnob {26 ""}
 addUserKnob {41 format l "output format" T Card3D1.format}
 addUserKnob {14 max_overscan l "max overscan" R 0 100}
 max_overscan 300
 addUserKnob {41 filter T Card3D1.filter}
 addUserKnob {41 clamp -STARTLINE T Card3D1.clamp}
 addUserKnob {41 black_outside l "black outside" -STARTLINE T Card3D1.black_outside}
 addUserKnob {41 motionblur T Card3D1.motionblur}
 addUserKnob {41 shutter l Shutter T Card3D1.shutter}
 addUserKnob {41 shutteroffset l "Shutter Offset" T Card3D1.shutteroffset}
 addUserKnob {41 shuttercustomoffset l "" -STARTLINE T Card3D1.shuttercustomoffset}
}
 Axis2 {
  inputs 0
  useMatrix true
  matrix {
      {{parent.DummyCam.world_matrix(ref_frame)} {parent.DummyCam.world_matrix(ref_frame)} {parent.DummyCam.world_matrix(ref_frame)} {parent.DummyCam.world_matrix(ref_frame)}}
      {{parent.DummyCam.world_matrix(ref_frame)} {parent.DummyCam.world_matrix(ref_frame)} {parent.DummyCam.world_matrix(ref_frame)} {parent.DummyCam.world_matrix(ref_frame)}}
      {{parent.DummyCam.world_matrix(ref_frame)} {parent.DummyCam.world_matrix(ref_frame)} {parent.DummyCam.world_matrix(ref_frame)} {parent.DummyCam.world_matrix(ref_frame)}}
      {{parent.DummyCam.world_matrix(ref_frame)} {parent.DummyCam.world_matrix(ref_frame)} {parent.DummyCam.world_matrix(ref_frame)} {parent.DummyCam.world_matrix(ref_frame)}}
    }
  name Axis1
  xpos -90
  ypos 291
 }
 Input {
  inputs 0
  name cam
  xpos -328
  ypos 77
  number 1
 }
 Dot {
  name Dot2
  xpos -294
  ypos 130
 }
set Naab5b000 [stack 0]
 Dot {
  name Dot1
  xpos -294
  ypos 398
 }
 Input {
  inputs 0
  name Input
  xpos 1
  ypos 77
 }
 Card3D {
  inputs 3
  xform_order RST
  translate {{"DummyCam.win_translate.u(ref_frame) * uniform_scale / 2"} {"DummyCam.win_translate.v(ref_frame) * uniform_scale / 2"} {-parent.distance}}
  rotate {0 0 {-DummyCam.winroll(ref_frame)}}
  scaling {{DummyCam.win_scale.u(ref_frame)} {DummyCam.win_scale.v(ref_frame)} 1}
  uniform_scale {{"DummyCam.projection_mode == 0 ? distance * DummyCam.haperture(ref_frame) / DummyCam.focal(ref_frame) : DummyCam.haperture(ref_frame) / DummyCam.focal(ref_frame)"}}
  motionblur 10
  shutteroffset centred
  name Card3D1
  xpos 1
  ypos 395
 }
 Crop {
  box {{"min(max(bbox.x, -max_overscan.w), width+max_overscan.w)"} {"min(max(bbox.y, -max_overscan.h), height+max_overscan.h)"} {"max(min(bbox.r, width+max_overscan.w), -max_overscan.w)"} {"max(min(bbox.t, height+max_overscan.h), -max_overscan.h)"}}
  crop false
  name Crop1
  xpos 1
  ypos 440
 }
 Output {
  name Output1
  xpos 1
  ypos 498
 }
 NoOp {
  inputs 0
  name ref_cam_input
  xpos -166
  ypos 125
 }
push $Naab5b000
 Switch {
  inputs 2
  which {{use_ref_cam}}
  name Switch1
  xpos -234
  ypos 188
 }
 Camera {
  projection_mode {{"\[expression \[value the_cam]projection_mode(\[value the_frame])]"}}
  focal {{"\[expression \[value the_cam]focal(\[value the_frame])]"}}
  haperture {{"\[expression \[value the_cam]haperture(\[value the_frame])]"}}
  vaperture {{"\[expression \[value the_cam]vaperture(\[value the_frame])]"}}
  near {{"\[expression \[value the_cam]near(\[value the_frame])]"}}
  far {{"\[expression \[value the_cam]far(\[value the_frame])]"}}
  win_translate {{"\[expression \[value the_cam]win_translate.u(\[value the_frame])]"} {"\[expression \[value the_cam]win_translate.v(\[value the_frame])]"}}
  win_scale {{"\[expression \[value the_cam]win_scale.u(\[value the_frame])]"} {"\[expression \[value the_cam]win_scale.v(\[value the_frame])]"}}
  winroll {{"\[expression \[value the_cam]winroll(\[value the_frame])]"}}
  focal_point {{"\[expression \[value the_cam]focal_point(\[value the_frame])]"}}
  fstop {{"\[expression \[value the_cam]fstop(\[value the_frame])]"}}
  name DummyCam
  help "DummyCam by Adrian Pueyo\n\nCamera that apart from the matrices also grabs all the \"Projection\" values from the upstream camera that it's connected to. Turns into a default Camera if no Camera connected. Can also be used inside of groups and gizmos, as many levels deep as you want :D All using live and super fast TCL.\n\nUpdated v1.1: Now grabs the correct frame too (thanks Erwan Leroy for the idea).\n\nadrianpueyo.com, 2019-2020"
  onCreate "n = nuke.thisNode()\nfor k in \[\"projection_mode\",\"focal\",\"haperture\",\"vaperture\",\"near\",\"far\",\"win_translate\",\"win_scale\",\"winroll\",\"focal_point\",\"fstop\"]:\n    n\[k].setFlag(0x0000000010000000)"
  xpos -224
  ypos 291
  addUserKnob {20 DummyCam l Defaults}
  addUserKnob {43 the_cam +INVISIBLE}
  the_cam "\[\n#DummyCam v1.1. Updated 5 April 2020.\nset starting_point \"this.input0\"\nset default \"this.d_\"\n\n# If cam has no inputs, return the default.\nif \{\[exists \$starting_point]\} \{\n    set x \[node \$starting_point]\n\} \{ \n    return \$default\n\}\n\nset finished 0\nwhile \{\$finished != 1\} \{\n\n    # First look for a Cam or Input or topnode.\n    while \{\[class \$x] != \"Camera4\" && \[class \$x] != \"Camera3\" && \[class \$x] != \"Camera2\" && \[class \$x] != \"Camera\" && \[class \$x] != \"Input\" && \$x != \[topnode \$x]\} \{\n        set x \[node \$x.input0]\n    \}\n\n    # Then, check if node is a cam (and return), and otherwise, if it's an input, see if the parent exists and move to it.\n    if \{\[class \$x]==\"Camera4\"||\[class \$x]==\"Camera3\"||\[class \$x]==\"Camera2\"||\[class \$x]==\"Camera\"\} \{\n        set x \[append x \".\"]\n        return \$x\n    \} \{ \n        if \{ \[class \$x]==\"Input\" \} \{ \n            set inp \"\$x.parent.input\"\n            set inputNum \[value \$x.number]\n            set inp \[append inp \$inputNum]\n            if \{ \[exists \$inp] \} \{\n                set x \[node \$inp]\n            \} \{ \n                set finished 1\n            \}\n        \} \{ \n            set finished 1\n        \}\n    \}\n\}\nreturn \$default\n]"
  addUserKnob {43 the_frame +INVISIBLE}
  the_frame "\[\nset the_camera \[string trimright \[value the_cam] .]\nif \{\[exists \$the_camera]\} \{\n    return \[value \$the_camera.frame]\n\} \{ \n    return \[frame]\n\}\n]"
  addUserKnob {4 d_projection_mode l projection M {perspective orthographic uv spherical ""}}
  addUserKnob {7 d_focal l "focal length" R 0 100}
  d_focal 50
  addUserKnob {7 d_haperture l "horiz aperture" R 0 50}
  d_haperture 24.576
  addUserKnob {7 d_vaperture l "vert aperture" R 0 50}
  d_vaperture 18.672
  addUserKnob {7 d_near l near R 0 10}
  d_near 0.1
  addUserKnob {7 d_far l far R 0 10000}
  d_far 10000
  addUserKnob {30 d_win_translate l "window translate"}
  addUserKnob {30 d_win_scale l "window scale"}
  d_win_scale {1 1}
  addUserKnob {7 d_winroll l "window roll" R 0 45}
  addUserKnob {7 d_focal_point l "focal distance" R 0 10}
  d_focal_point 2
  addUserKnob {7 d_fstop l fstop R 0 30}
  d_fstop 16
  addUserKnob {26 version l " " t "Updated 5 April 2020" T "<span style=\"color:#666\"><br/><b>DummyCam v1.1</b> - <a href=\"http://www.adrianpueyo.com\" style=\"color:#666;text-decoration: none;\">adrianpueyo.com</a>, 2019-2020</span>"}
 }
end_group

```