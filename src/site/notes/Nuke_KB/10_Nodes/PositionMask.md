---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/position-mask/","tags":["nuke","node","pass","relight","grade"]}
---


# PositionMask

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
     name Position_Mask1
     help "Position Mask Sphere\n\nCreate a mask using the render of the Position pass. \nIt will have a shape of a circle becouse a sphere is use to create it.\n"
     knobChanged "\nif nuke.thisKnob().name() == '_Picker':\n\txPos, yPos =  nuke.thisNode().knob('_Picker').value()\n\twith nuke.thisNode():\n\t\tnode = nuke.toNode('Shuffle_input_channel')\n\t\tr = node.sample('red', xPos, yPos)\n\t\tg = node.sample('green', xPos, yPos)\n\t\tb = node.sample('blue', xPos, yPos)\n\n\t\tnode = nuke.toNode('Sphere')\n\t\tnode.knob('center').setValue(\[r,g,b]) \n\nif nuke.thisKnob().name() in ('matte', 'showPanel'):\n\tinputT = nuke.exists('Alpha')\n\tmode = nuke.thisNode()\['matte'].value()\n\tif mode == 'Position Input':\n\t\tif str(inputT)== 'True':\n\t\t\tn = nuke.toNode('Alpha')\n\t\t\tnuke.delete(n)\n\telif mode == 'Alpha (New Input)':\n\t\tif str(inputT)== 'False':\n\t\t\tnuke.message('Add New Input in Node')\n\t\t\ti = nuke.createNode('Input', inpanel=False )\n\t\t\ti\['name'].setValue('Alpha')\n\t\t\tn = nuke.toNode('Switch2')\n\t\t\tn.setInput(0,i)\n\nnode = nuke.thisNode()\nknob = nuke.thisKnob()\nif knob.name() in ('visualise', 'showPanel'):\n    mode = node\['visualise'].value()\n    if mode == 'None':\n        node\['point_detail'].setVisible(False)\n        node\['point_size'].setVisible(False)\n    else:\n        node\['point_detail'].setVisible(True)\n        node\['point_size'].setVisible(True)\n"
     tile_color 0xdda92aff
     selected true
     xpos -9921
     ypos 432
     addUserKnob {20 PMS l "Position Mask"}
     addUserKnob {41 in l "Position Channel" t "Choose the position pass channel." T Shuffle_input_channel.in}
     addUserKnob {6 premult l "(Un)Premult by     -->  " t "The Position Pass is divide by the Alpha channel before being processed, and multiplied again afterwards. This can improve the texturing of anti-aliased edges." +STARTLINE}
     premult true
     addUserKnob {4 matte l " Alpha from " t "Choose the Alpha channel.\nAlpha is usefull for a better result with clean edge (UnPremult / Premult).\n\nPosition Input: \nAutomatic pick the .a from the Input.\n\nAlpha (New Input):\nPick the .a from an other Input." -STARTLINE M {"Position Input" "Alpha (New Input)" ""}}
     addUserKnob {26 ""}
     addUserKnob {12 _Picker l Position t "Set a position.\nIt will be the center of the 3D mask."}
     _Picker {1684 1252}
     addUserKnob {6 overlay l "Overlay      Color:" -STARTLINE}
     overlay true
     addUserKnob {6 r l "R  " -STARTLINE}
     r true
     addUserKnob {6 g l "G  " -STARTLINE}
     addUserKnob {6 b l B -STARTLINE}
     addUserKnob {26 S01 l " " T " "}
     addUserKnob {41 center_1 l INVISIBLE t "Pick a value. \nIt will be the center of the sphere." +INVISIBLE T Sphere.center}
     addUserKnob {26 ""}
     addUserKnob {4 shape l Shape M {Sphere Cube}}
     addUserKnob {4 falloff l "             Falloff" t "Falloff profile of the feathered edge." -STARTLINE M {Linear Smoothstep Cubic "Inverse Cubic" "" "" "" "" "" "" "" "" "" "" "" ""}}
     addUserKnob {26 S06 l " "}
     addUserKnob {7 uni_scale l "<font color=\"white\">Global <font color=\"grey\">Scale" R 0 100}
     uni_scale 10
     addUserKnob {7 radius l "<font color=\"white\">Out<font color=\"grey\"> Scale" t "Adjust feather." R 0 100}
     radius 22
     addUserKnob {7 inner_radius l "<font color=\"white\">In <font color=\"grey\">Scale" t "Adjust inner scale." R 0 100}
     addUserKnob {26 S05 l " " T " "}
     addUserKnob {13 offset l Translate}
     offset {0 0.6 0}
     addUserKnob {13 rotate l Rotate}
     addUserKnob {13 scalediv l Scale}
     scalediv {1 1 1}
     addUserKnob {26 S03 l " " T " "}
     addUserKnob {26 ""}
     addUserKnob {4 visualise l "Visualize in 3D" t "Build Sphere and Point Cloud in 3D Space." M {None Active "" "" "" ""}}
     addUserKnob {7 point_detail l "Point Detail" +HIDDEN}
     point_detail 1
     addUserKnob {7 point_size l "Point Size" +HIDDEN R 0 10}
     point_size 4
     addUserKnob {26 ""}
     addUserKnob {7 opacity l Opacity}
     opacity 1
     addUserKnob {1 output l INVISIBLE +INVISIBLE}
     output "\[if \{\[value visualise]!=\"None\"\} \{return \"\[knob this.tile_color 0xa30000ff]\"\}  \{return \"\[knob this.tile_color 0xdda92aff]\"\}]\[value shape]"
     addUserKnob {26 by1 l " " T " "}
     addUserKnob {26 by2 l " " T "                                                                                               "}
     addUserKnob {26 CGEV l " " t "\nEn cas de probleme, contacter Gaetan Baldy sur le chat\n" -STARTLINE T "<font color=\"#1C1C1C\"> v04 - CGEV - 2016"}
    }
     Input {
      inputs 0
      name Position
      xpos -889
      ypos -1005
     }
     Dot {
      name Dot9
      xpos -855
      ypos -854
     }
    set Nc13ff830 [stack 0]
    push 0
     Switch {
      inputs 2
      which {{!matte i}}
      name Switch2
      xpos -1235
      ypos -858
     }
     NoOp {
      name AlphaCheck
      xpos -1235
      ypos -803
      addUserKnob {20 User}
      addUserKnob {6 alpha +STARTLINE}
      alpha {{"\[python \"len(\\\[n for n in nuke.channels(nuke.thisNode().input(0)) if n.find(\\\".a\\\") != -1])>0\"]" i}}
     }
     AddChannels {
      channels rgba
      name AddAlpha
      xpos -1235
      ypos -779
     }
     Dot {
      name Dot11
      xpos -1201
      ypos -697
     }
    set N25d5b4d0 [stack 0]
     Dot {
      name Dot12
      xpos -1201
      ypos 610
     }
    push $N25d5b4d0
    push $Nc13ff830
     Dot {
      name Dot1
      xpos -561
      ypos -856
     }
    set N35bd0700 [stack 0]
     Copy {
      inputs 2
      from0 rgba.alpha
      to0 rgba.alpha
      name Copy1
      xpos -594
      ypos -708
     }
    set N323effc0 [stack 0]
     Unpremult {
      name Unpremult1
      xpos -489
      ypos -609
      disable {{!AlphaCheck.alpha i}}
     }
    push $N323effc0
     Switch {
      inputs 2
      which {{parent.premult}}
      name Switch5
      xpos -594
      ypos -517
     }
     Dot {
      name Dot5
      xpos -560
      ypos 54
     }
    set Na7e8820 [stack 0]
     Dot {
      name Dot6
      xpos -424
      ypos 54
     }
     Dot {
      name Dot7
      xpos -424
      ypos 519
     }
    push $N35bd0700
     Shuffle {
      name Shuffle1
      xpos -368
      ypos -860
      disable {{!parent.visualise}}
     }
    push $N25d5b4d0
    push $Nc13ff830
     Shuffle {
      in rgb
      name Shuffle_input_channel
      xpos -889
      ypos -784
     }
     Copy {
      inputs 2
      from0 rgba.alpha
      to0 rgba.alpha
      name Copy2
      xpos -889
      ypos -705
     }
    set N11c14880 [stack 0]
     Unpremult {
      name Unpremult2
      xpos -1001
      ypos -600
      disable {{!AlphaCheck.alpha i}}
     }
    push $N11c14880
     Switch {
      inputs 2
      which {{parent.premult}}
      name Switch9
      xpos -889
      ypos -513
     }
     Dot {
      name Dot8
      xpos -855
      ypos -439
     }
    set N3a51d400 [stack 0]
     PositionToPoints {
      inputs 2
      display textured
      selectable false
      render_mode textured
      cast_shadow false
      receive_shadow false
      detail {{point_detail}}
      pointSize {{point_size}}
      name PositionToPoints1
      selected true
      xpos -368
      ypos -443
      disable {{!parent.visualise}}
     }
     Dot {
      name Dot10
      xpos -334
      ypos -231
     }
     Constant {
      inputs 0
      color {0 0 1 0.5}
      name Constant1
      xpos 177
      ypos -531
      disable {{!parent.visualise}}
      postage_stamp false
     }
    set N2ee2eaf0 [stack 0]
     Cube {
      selectable false
      render_mode off
      cast_shadow false
      receive_shadow false
      rows 1
      columns 1
      separate_faces false
      cube {-1 -1 -1 1 1 1}
      translate {{parent.Cube1.translate} {parent.Cube1.translate} {parent.Cube1.translate}}
      rotate {{parent.rotate.x} {parent.rotate.y} {parent.rotate.z}}
      scaling {{Sphere.scale.x} {Sphere.scale.y} {Sphere.scale.z}}
      uniform_scale {{"parent.radius > parent.inner_radius ? parent.inner_radius:parent.radius"}}
      name Cube2
      xpos 177
      ypos -505
      disable {{!parent.visualise}}
     }
     Constant {
      inputs 0
      color {1 0 0 0.5}
      name Constant2
      xpos -197
      ypos -527
      disable {{!parent.visualise}}
      postage_stamp false
     }
    set Ndc57ceb0 [stack 0]
     Cube {
      selectable false
      render_mode off
      cast_shadow false
      receive_shadow false
      rows 1
      columns 1
      separate_faces false
      cube {-1 -1 -1 1 1 1}
      translate {{Sphere.center.r+parent.offset.x} {Sphere.center.g+parent.offset.y} {Sphere.center.b+parent.offset.z}}
      rotate {{parent.rotate.x} {parent.rotate.y} {parent.rotate.z}}
      scaling {{Sphere.scale.x} {Sphere.scale.y} {Sphere.scale.z}}
      uniform_scale {{parent.radius}}
      name Cube1
      xpos -71
      ypos -501
      disable {{!parent.visualise}}
     }
     Scene {
      inputs 2
      render_mode off
      name Scene3
      xpos 187
      ypos -406
      disable {{!parent.visualise}}
     }
    push $Ndc57ceb0
     Sphere {
      selectable false
      render_mode off
      cast_shadow false
      receive_shadow false
      rows 20
      columns 20
      radius {{parent.radius}}
      translate {{Sphere.center.r+parent.offset.x} {Sphere.center.g+parent.offset.y} {Sphere.center.b+parent.offset.z}}
      rotate {{parent.rotate.x} {parent.rotate.y} {parent.rotate.z}}
      scaling {{Sphere.scale.x} {Sphere.scale.y} {Sphere.scale.z}}
      name Sphere2
      xpos -197
      ypos -501
      disable {{!parent.visualise}}
     }
    push $N2ee2eaf0
     Sphere {
      selectable false
      render_mode off
      cast_shadow false
      receive_shadow false
      radius {{"parent.radius > parent.inner_radius ? parent.inner_radius:parent.radius"}}
      translate {{Sphere.center.r+parent.offset.x} {Sphere.center.g+parent.offset.y} {Sphere.center.b+parent.offset.z}}
      rotate {{parent.rotate.x} {parent.rotate.y} {parent.rotate.z}}
      scaling {{Sphere.scale.x} {Sphere.scale.y} {Sphere.scale.z}}
      name Sphere1
      xpos 54
      ypos -504
      disable {{!parent.visualise}}
     }
     Scene {
      inputs 2
      selectable false
      name Scene2
      xpos -187
      ypos -406
      disable {{!parent.visualise}}
     }
     Switch {
      inputs 2
      which {{shape}}
      name Switch4
      xpos -7
      ypos -386
     }
     Scene {
      inputs 2
      selectable false
      name Scene1
      xpos 3
      ypos -255
      disable {{!parent.visualise}}
     }
    push $N3a51d400
     Expression {
      temp_name0 x
      temp_expr0 abs(normX.x*(center.r+parent.offset.x-r)+normX.y*(center.g+parent.offset.y-g)+normX.z*(center.b+parent.offset.z-b))
      temp_name1 y
      temp_expr1 abs(normY.x*(center.r+parent.offset.x-r)+normY.y*(center.g+parent.offset.y-g)+normY.z*(center.b+parent.offset.z-b))
      temp_name2 z
      temp_expr2 abs(normZ.x*(center.r+parent.offset.x-r)+normZ.y*(center.g+parent.offset.y-g)+normZ.z*(center.b+parent.offset.z-b))
      channel0 none
      channel1 none
      channel2 none
      channel3 {-rgba.red -rgba.green -rgba.blue rgba.alpha}
      expr3 1-max(x/scale.x/parent.radius*2,y/scale.y/parent.radius*2,z/scale.z/parent.radius*2)/2
      name Cube
      xpos -841
      ypos -349
      cached true
      addUserKnob {20 User}
      addUserKnob {18 center l Center}
      center {{parent.Sphere.center} {parent.Sphere.center} {parent.Sphere.center}}
      addUserKnob {6 center_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {13 rad}
      rad {{radians(parent.rotate.x)} {radians(parent.rotate.y)} {radians(parent.rotate.z)}}
      addUserKnob {13 normX}
      normX {{cos(rad.z)*(cos(rad.y))} {sin(rad.z)*(cos(rad.y))} {-sin(rad.y)}}
      addUserKnob {13 normY}
      normY {{cos(rad.z)*(sin(rad.y)*(-sin(rad.x)))-sin(rad.z)*(cos(rad.x))} {sin(rad.z)*(sin(rad.y)*(-sin(rad.x)))-cos(rad.z)*(cos(rad.x))} {(cos(rad.y)*(-sin(rad.x)))}}
      addUserKnob {13 normZ}
      normZ {{cos(rad.z)*(sin(rad.y)*cos(rad.x))-sin(rad.z)*sin(rad.x)} {cos(rad.z)*(sin(rad.y)*cos(rad.x))+cos(rad.z)*sin(rad.x)} {cos(rad.y)*cos(rad.x)}}
      addUserKnob {26 ""}
      addUserKnob {13 scale}
      scale {{scalediv.x/10*parent.uni_scale} {scalediv.y/10*parent.uni_scale} {scalediv.z/10*parent.uni_scale}}
     }
    push $N3a51d400
     Expression {
      temp_name0 x
      temp_expr0 abs(normX.x*(center.r+parent.offset.x-r)+normX.y*(center.g+parent.offset.y-g)+normX.z*(center.b+parent.offset.z-b))
      temp_name1 y
      temp_expr1 abs(normY.x*(center.r+parent.offset.x-r)+normY.y*(center.g+parent.offset.y-g)+normY.z*(center.b+parent.offset.z-b))
      temp_name2 z
      temp_expr2 abs(normZ.x*(center.r+parent.offset.x-r)+normZ.y*(center.g+parent.offset.y-g)+normZ.z*(center.b+parent.offset.z-b))
      channel0 none
      channel1 none
      channel2 none
      channel3 {-rgba.red -rgba.green -rgba.blue rgba.alpha}
      expr3 "r == 0 && g == 0 && b == 0?0:(scale.x != 1 || scale.y != 1 || scale.z != 1?1-(sqrt(pow2(x)/pow2(scale.x)+pow2(y)/pow2(scale.y)+pow2(z)/pow2(scale.z))/parent.radius):1-(sqrt(pow2(x)+pow2(y)+pow2(z))/parent.radius))"
      name Sphere
      xpos -951
      ypos -348
      cached true
      addUserKnob {20 User}
      addUserKnob {18 center l Center}
      center {154.52565 188.4417877 -64.84392548}
      addUserKnob {6 center_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
      addUserKnob {26 ""}
      addUserKnob {13 rad}
      rad {{radians(parent.rotate.x)} {radians(parent.rotate.y)} {radians(parent.rotate.z)}}
      addUserKnob {13 normX}
      normX {{cos(rad.z)*(cos(rad.y))} {sin(rad.z)*(cos(rad.y))} {-sin(rad.y)}}
      addUserKnob {13 normY}
      normY {{cos(rad.z)*(sin(rad.y)*(-sin(rad.x)))-sin(rad.z)*(cos(rad.x))} {sin(rad.z)*(sin(rad.y)*(-sin(rad.x)))-cos(rad.z)*(cos(rad.x))} {(cos(rad.y)*(-sin(rad.x)))}}
      addUserKnob {13 normZ}
      normZ {{cos(rad.z)*(sin(rad.y)*cos(rad.x))-sin(rad.z)*sin(rad.x)} {cos(rad.z)*(sin(rad.y)*cos(rad.x))+cos(rad.z)*sin(rad.x)} {cos(rad.y)*cos(rad.x)}}
      addUserKnob {26 ""}
      addUserKnob {13 scale}
      scale {{scalediv.x/10*parent.uni_scale} {scalediv.y/10*parent.uni_scale} {scalediv.z/10*parent.uni_scale}}
     }
     Switch {
      inputs 2
      which {{shape}}
      name Switch3
      xpos -889
      ypos -259
     }
     Grade {
      channels alpha
      whitepoint {{"1-(min(parent.inner_radius, parent.radius)*(1/parent.radius))"}}
      white_clamp true
      name inner_Radius
      label "\[if \{\[value reverse]==\"false\"\} \{return \"\[knob this.icon -]\"\} \{return \"\[knob this.icon Reverse]\"\}]\[value icon]"
      xpos -889
      ypos -186
      icon -
     }
     Dot {
      name Dot3
      xpos -855
      ypos -95
     }
    set N2f018f50 [stack 0]
     ScanlineRender {
      inputs 2
      motion_vectors_type velocity
      name ScanlineRender1
      xpos -7
      ypos -101
      disable true
     }
     Dot {
      name Dot2
      xpos 27
      ypos 384
     }
    push $N2f018f50
     Expression {
      expr3 a+(a-pow(a,2))
      name Inv_Cubic
      xpos -735
      ypos 33
     }
    push $N2f018f50
     Expression {
      expr3 pow(a,2)
      name Cubic
      xpos -835
      ypos 32
     }
    push $N2f018f50
     Expression {
      expr3 smoothstep(0,1,a)
      name Smoothstep
      xpos -940
      ypos 30
     }
    push $N2f018f50
     Expression {
      name Linear
      xpos -1050
      ypos 30
     }
     Switch {
      inputs 4
      which {{parent.falloff i}}
      name Switch1
      xpos -888
      ypos 157
     }
    set Ne2bf69e0 [stack 0]
     Dot {
      name Dot4
      xpos -854
      ypos 244
     }
    push $Ne2bf69e0
    push $Na7e8820
     Grade {
      inputs 1+1
      multiply {{parent.r} {parent.g} {parent.b} 0}
      name Grade1
      xpos -594
      ypos 157
      disable {{!overlay}}
     }
     Copy {
      inputs 2
      from0 rgba.alpha
      to0 rgba.alpha
      name Copy3
      xpos -594
      ypos 234
     }
     Merge2 {
      inputs 2
      bbox B
      name Merge1
      xpos -594
      ypos 380
      disable true
     }
     CopyBBox {
      inputs 2
      name CopyBBox1
      xpos -594
      ypos 515
     }
    set N5c5569a0 [stack 0]
     Multiply {
      inputs 1+1
      value 0
      invert_mask true
      name Multiply3
      xpos -709
      ypos 598
      disable {{!AlphaCheck.alpha i}}
     }
    push $N5c5569a0
     Switch {
      inputs 2
      which {{parent.premult}}
      name Switch10
      xpos -594
      ypos 686
     }
     Multiply {
      channels alpha
      value {{parent.opacity i}}
      name Multiply2
      xpos -594
      ypos 764
     }
     Output {
      name FranklinVFX
      xpos -594
      ypos 858
     }
    end_group
```
