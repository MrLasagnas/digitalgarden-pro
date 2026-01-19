---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/og-3-d-mask-ramp/","tags":["nuke","node","mask","grade"]}
---


# OG 3D mask ramp

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
    name OG_3DMaskRamp1
    tile_color 0x656565ff
    note_font_color 0xfefefeff
    selected true
    xpos -15106
    ypos 12197
    addUserKnob {20 m_3dmaskRamp l M_3DMaskRamp}
    addUserKnob {41 in l Channel T Shuffle_Input_Channels.in}
    addUserKnob {4 matte l Matte M {Source Alpha "" ""}}
    matte Alpha
    addUserKnob {6 unpremultiply l Unpremult -STARTLINE}
    unpremultiply true
    addUserKnob {26 ""}
    addUserKnob {4 axis l Axis M {- X Y Z ""}}
    axis Y
    addUserKnob {18 front l White}
    front {69.29559326 96.93513489 73.28577423}
    addUserKnob {6 front_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
    addUserKnob {18 back l Black}
    back {69.08917236 30.38646126 73.34415436}
    addUserKnob {6 back_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
    addUserKnob {6 center_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
    addUserKnob {4 falloff l Falloff M {Linear Smoothstep Cubic "Inverse Cubic" "" ""}}
    addUserKnob {6 keepRGBA l "Keep RGBA only" +STARTLINE}
    keepRGBA true
    addUserKnob {6 visual l "Visualize in 3D" +STARTLINE}
    }
    Input {
    inputs 0
    name Alpha
    xpos -627
    ypos -877
    number 1
    }
    Dot {
    name Dot8
    xpos -593
    ypos -761
    }
    Input {
    inputs 0
    name Position
    xpos -469
    ypos -875
    }
    Shuffle {
    name Shuffle_Input_Channels
    xpos -469
    ypos -807
    }
    ShuffleCopy {
    inputs 2
    name Shuffle_Input_Alpha
    xpos -469
    ypos -765
    disable {{!parent.matte}}
    }
    Dot {
    name Dot9
    xpos -435
    ypos -696
    }
    Unpremult {
    name Unpremult1
    xpos -469
    ypos -637
    disable {{!parent.unpremultiply}}
    }
    set N22bab500 [stack 0]
    Dot {
    name Dot3
    xpos -435
    ypos -520
    }
    set N22bb3b90 [stack 0]
    Expression {
    channel0 alpha
    expr0 clamp((b-parent.back.b)/(parent.front.b-parent.back.b),0,1)
    channel1 none
    channel2 none
    name Z
    xpos -368
    ypos -447
    }
    push $N22bb3b90
    Expression {
    channel0 alpha
    expr0 clamp((g-parent.back.g)/(parent.front.g-parent.back.g),0,1)
    channel1 none
    channel2 none
    name Y
    xpos -469
    ypos -445
    }
    push $N22bb3b90
    Expression {
    channel0 alpha
    expr0 clamp((r-parent.back.r)/(parent.front.r-parent.back.r),0,1)
    channel1 none
    channel2 none
    name X
    xpos -584
    ypos -442
    }
    push $N22bb3b90
    Expression {
    temp_name0 DistF
    temp_expr0 "sqrt(pow2(parent.back.r - r) + pow2(parent.back.g - g) + pow2(parent.back.b - b))"
    temp_name1 DistT
    temp_expr1 "sqrt(pow2(parent.front.r - r) + pow2(parent.front.g - g) + pow2(parent.front.b - b))"
    temp_name2 DistFT
    temp_expr2 "sqrt(pow2(parent.back.r - parent.front.r) + pow2(parent.back.g - parent.front.g) + pow2(parent.back.b - parent.front.b))"
    temp_name3 F
    temp_expr3 "acos(clamp((pow2(DistF) + pow2(DistFT) - pow2(DistT))/(2 * DistF * DistFT), -1, 1))"
    channel0 none
    channel1 none
    channel2 none
    expr3 "clamp((DistF * cos(F)) / DistFT)"
    name Gradient_Free
    xpos -699
    ypos -444
    addUserKnob {20 User}
    addUserKnob {18 From}
    From {247.2421875 19.44228363 -15.71759415}
    addUserKnob {6 From_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
    addUserKnob {18 To}
    To {237.6498108 1.51267755 -17.16628075}
    addUserKnob {6 To_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
    addUserKnob {6 ReplaceRGB l "Replace RGB / Keep original (generate matte in alpha only)" +STARTLINE}
    addUserKnob {6 IgnoreByAlpha l "Ignore outside by alpha" +STARTLINE}
    }
    Switch {
    inputs 4
    which {{parent.axis}}
    name Switch1
    xpos -469
    ypos -333
    }
    Dot {
    name Dot12
    xpos -435
    ypos -256
    }
    set N234d0b60 [stack 0]
    Expression {
    expr3 "a+(a-pow(a, 2))"
    name Inv_Cubic
    xpos -287
    ypos -201
    }
    push $N234d0b60
    Expression {
    expr3 "pow(a, 2)"
    name Cubic
    xpos -410
    ypos -201
    }
    push $N234d0b60
    Expression {
    expr3 "smoothstep(0, 1, a)"
    name Smoothstep
    xpos -538
    ypos -201
    }
    push $N234d0b60
    NoOp {
    name Linear
    xpos -667
    ypos -198
    }
    Switch {
    inputs 4
    which {{parent.falloff}}
    name Switch2
    xpos -469
    ypos -136
    }
    Dot {
    name Dot2
    xpos -435
    ypos -45
    }
    set N2350e270 [stack 0]
    Dot {
    name Dot6
    xpos -553
    ypos -45
    }
    Dot {
    name Dot5
    xpos -553
    ypos 109
    }
    push $N2350e270
    Shuffle {
    inputs 0
    red black
    green black
    blue white
    alpha white
    name Shuffle3
    xpos 393
    ypos -878
    disable {{!parent.visual}}
    }
    Sphere {
    selectable false
    cast_shadow false
    receive_shadow false
    rows 10
    columns 10
    translate {{parent.back.r} {parent.back.g} {parent.back.b}}
    name Sphere2
    xpos 393
    ypos -837
    disable {{!parent.visual}}
    }
    Shuffle {
    inputs 0
    red black
    green white
    blue black
    alpha white
    name Shuffle2
    xpos 107
    ypos -884
    disable {{!parent.visual}}
    }
    Sphere {
    selectable false
    cast_shadow false
    receive_shadow false
    rows 10
    columns 10
    translate {{parent.front.r} {parent.front.g} {parent.front.b}}
    name Sphere1
    xpos 107
    ypos -843
    disable {{!parent.visual}}
    }
    push $N22bab500
    Dot {
    name Dot1
    xpos -162
    ypos -633
    }
    set N23564000 [stack 0]
    Shuffle {
    red white
    green black
    blue black
    alpha white
    name Shuffle1
    xpos 28
    ypos -497
    disable {{!parent.visual}}
    }
    set N23568d60 [stack 0]
    push $N23564000
    PositionToPoints {
    inputs 2
    display textured
    selectable false
    render_mode textured
    cast_shadow false
    receive_shadow false
    detail 0.006
    pointSize 4
    name PositionToPoints1
    xpos 28
    ypos -637
    disable {{!parent.visual x1081 1}}
    }
    Axis2 {
    inputs 0
    display solid
    selectable false
    translate {{parent.back.r} {parent.back.g} {parent.back.b}}
    name black
    gl_color 0xffff
    xpos 303
    ypos -852
    disable {{!parent.visual}}
    addUserKnob {20 Ivy}
    addUserKnob {22 ivy_documentation l "Ivy Documentation" t "Open IvyTab documentation page in your web browser" T "__import__('dnnuke.utils.asst.common', fromlist=\['openDocsName']).openDocsName('IvyTab')" +STARTLINE}
    addUserKnob {26 divider4 l "" +STARTLINE}
    addUserKnob {1 _ivyVals l "" +STARTLINE +HIDDEN}
    _ivyVals "\{'ivy_job': '', 'ivy_usemanualuri': False, 'leafname': '', 'ivy_vnum': 0, 'ivy_twignametags': \{\}, 'ivy_shot': '', 'ivy_regex': False, 'ivy_type': '', 'ivy_versionquery': '', 'spider_uri': ''\}"
    addUserKnob {52 ivyTab l "" -STARTLINE T "__import__('nukescripts').panels.WidgetKnob(__import__('dnnuke.core.ivy.tab.widget.query', fromlist=\['getBoundKnob']).getBoundKnob(nuke.thisNode()))"}
    addUserKnob {1 _ivyFile l "" +STARTLINE +HIDDEN +INVISIBLE}
    addUserKnob {78 _expressions l "" -STARTLINE +HIDDEN +INVISIBLE n 1}
    _expressions {{curve}}
    addUserKnob {26 divider0 l "" +STARTLINE}
    addUserKnob {20 dbinfo l "DB Info" n 1}
    dbinfo 0
    addUserKnob {1 twig_dnuuid l "Twig Id"}
    addUserKnob {1 stalk_dnuuid l "Stalk Id"}
    addUserKnob {1 twigtype_dnuuid l "Twig Type Id"}
    addUserKnob {1 leaf_dnuuid l "Leaf Id"}
    addUserKnob {26 divider1 l "" +STARTLINE}
    addUserKnob {1 twigtype l "Twig Type"}
    addUserKnob {1 leaf_label l Leaf}
    addUserKnob {1 ivy_version l Version}
    addUserKnob {43 ivy_notes l Notes}
    addUserKnob {26 divider2 l "" +STARTLINE}
    addUserKnob {1 build_label l "Build Label"}
    addUserKnob {20 dbinfoEndGroup l "DB Info" n -1}
    }
    Axis2 {
    inputs 0
    display solid+wireframe
    selectable false
    translate {{parent.front.r} {parent.front.g} {parent.front.b}}
    name white
    gl_color 0xff00ff
    xpos 23
    ypos -858
    disable {{!parent.visual}}
    addUserKnob {20 Ivy}
    addUserKnob {22 ivy_documentation l "Ivy Documentation" t "Open IvyTab documentation page in your web browser" T "__import__('dnnuke.utils.asst.common', fromlist=\['openDocsName']).openDocsName('IvyTab')" +STARTLINE}
    addUserKnob {26 divider4 l "" +STARTLINE}
    addUserKnob {1 _ivyVals l "" +STARTLINE +HIDDEN}
    _ivyVals "\{'ivy_job': '', 'ivy_usemanualuri': False, 'leafname': '', 'ivy_vnum': 0, 'ivy_twignametags': \{\}, 'ivy_shot': '', 'ivy_regex': False, 'ivy_type': '', 'ivy_versionquery': '', 'spider_uri': ''\}"
    addUserKnob {52 ivyTab l "" -STARTLINE T "__import__('nukescripts').panels.WidgetKnob(__import__('dnnuke.core.ivy.tab.widget.query', fromlist=\['getBoundKnob']).getBoundKnob(nuke.thisNode()))"}
    addUserKnob {1 _ivyFile l "" +STARTLINE +HIDDEN +INVISIBLE}
    addUserKnob {78 _expressions l "" -STARTLINE +HIDDEN +INVISIBLE n 1}
    _expressions {{curve}}
    addUserKnob {26 divider0 l "" +STARTLINE}
    addUserKnob {20 dbinfo l "DB Info" n 1}
    dbinfo 0
    addUserKnob {1 twig_dnuuid l "Twig Id"}
    addUserKnob {1 stalk_dnuuid l "Stalk Id"}
    addUserKnob {1 twigtype_dnuuid l "Twig Type Id"}
    addUserKnob {1 leaf_dnuuid l "Leaf Id"}
    addUserKnob {26 divider1 l "" +STARTLINE}
    addUserKnob {1 twigtype l "Twig Type"}
    addUserKnob {1 leaf_label l Leaf}
    addUserKnob {1 ivy_version l Version}
    addUserKnob {43 ivy_notes l Notes}
    addUserKnob {26 divider2 l "" +STARTLINE}
    addUserKnob {1 build_label l "Build Label"}
    addUserKnob {20 dbinfoEndGroup l "DB Info" n -1}
    }
    Scene {
    inputs 5
    name Scene1
    xpos 220
    ypos -657
    disable {{!parent.visual}}
    }
    push $N23568d60
    ScanlineRender {
    inputs 2
    transparency false
    ztest_enabled false
    filter impulse
    max_tessellation 1
    motion_vectors_type off
    MB_channel none
    name ScanlineRender1
    xpos 210
    ypos -497
    disable true
    }
    Dot {
    name Dot7
    xpos 244
    ypos 39
    }
    ShuffleCopy {
    inputs 2
    in2 none
    red red
    green green
    blue blue
    name ShuffleCopy1
    xpos -469
    ypos 35
    }
    CopyBBox {
    inputs 2
    name CopyBBox1
    xpos -469
    ypos 105
    }
    Remove {
    operation keep
    channels rgba
    name Remove1
    selected true
    xpos -469
    ypos 166
    disable {{!parent.keepRGBA}}
    }
    Output {
    name Output1
    xpos -469
    ypos 286
    }
    end_group
```
