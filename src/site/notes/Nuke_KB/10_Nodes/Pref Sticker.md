---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/pref-sticker/","tags":["nuke","node","3D","mask"]}
---


# Pref Sticker

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
    name gRefSticker
    knobChanged "n = nuke.thisNode()\nk = nuke.thisKnob()\n\nif k.name() == 'ViewUnwrapedUVs':\n    n\['textureOpacity'].setVisible(True) if k.value() else n\['textureOpacity'].setVisible(False)\n"
    tile_color 0xbf8858ff
    note_font Verdana
    selected true
    xpos -4406
    ypos -2288
    addUserKnob {20 tab l prefStick}
    addUserKnob {6 useGPUIfAvailable l "Use GPU if available" +STARTLINE}
    useGPUIfAvailable true
    addUserKnob {41 vectorize l "Vectorize on CPU" -STARTLINE T BlinkScript2.vectorize}
    addUserKnob {26 ""}
    addUserKnob {20 help_1 l Help n 1}
    help_1 0
    addUserKnob {26 text_1 l "" +STARTLINE T "     1 If your pref coordinates are far from the world center, use the picker to select a point, \n         this will do an offset.\n\n     2 Select the projection direction.\n\n     3 Use the unwrap function to see the UVs and place your texture on it.\n\n     4 You can translate, scale and rotate the projection to better place the image.\n\n     5 Use the pref matte to mask your texture.\n\n"}
    addUserKnob {20 endGroup n -1}
    addUserKnob {22 getCam l "Get Camera" +INVISIBLE T "thisPrefStick = nuke.thisNode()\n\nx = nuke.Root()\nx.begin()\n\nif nuke.selectedNode().Class() == \"Camera2\" or nuke.selectedNode().Class() == \"Camera3\":\n    targetCam = nuke.selectedNode()\n    print (targetCam)\n    haperture = targetCam.knob('haperture').value()\n    vaperture = targetCam.knob('vaperture').value()\n    thisPrefStick.knob('uv').setValue(haperture, 0)\n    thisPrefStick.knob('uv').setValue(vaperture, 1)\n\nelse:\n    nuke.message('select a camera!')\nx.end()" +STARTLINE}
    addUserKnob {30 uv -STARTLINE +INVISIBLE}
    uv {36 24}
    addUserKnob {26 space1 l "  " T "  "}
    addUserKnob {41 in l "Pref Layer" T ShufflePref.in}
    addUserKnob {26 space2 l "  " T "  "}
    addUserKnob {19 pref_pick l "Pref Offset" t "Use this if your pref is far from the center of the world"}
    pref_pick {0 0 0 0}
    addUserKnob {6 pref_pick_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
    addUserKnob {6 color_rgba_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
    addUserKnob {4 directionPull l Direction M {XY XZ YZ}}
    directionPull YZ
    addUserKnob {6 invert_u l "Invert U" -STARTLINE}
    addUserKnob {6 invert_v l "Invert V" -STARTLINE}
    addUserKnob {6 checkChecker l "Check UVs" +INVISIBLE +STARTLINE}
    addUserKnob {6 ViewUnwrapedUVs l "View Unwraped UVs" +STARTLINE}
    addUserKnob {7 textureOpacity l "Texture Opacity" +HIDDEN}
    textureOpacity 1
    addUserKnob {26 ""}
    addUserKnob {7 oX l "Offset X" R -1 1}
    addUserKnob {7 oY l "Offset Y" R -1 1}
    addUserKnob {14 scale R 0 100}
    scale 1
    addUserKnob {13 rotate}
    addUserKnob {26 ""}
    addUserKnob {20 group_1 l "Pref Mask" n 1}
    group_1 0
    addUserKnob {6 PrefMatteMask +STARTLINE}
    addUserKnob {6 show_mask l "Show Mask" -STARTLINE}
    addUserKnob {6 show l "Show Pref" -STARTLINE}
    addUserKnob {4 prefMatteForm l Shape M {Sphere Cube "" "" "" "" ""}}
    addUserKnob {19 matte_pick l "matte pick"}
    matte_pick {0 0 0 0}
    addUserKnob {6 matte_pick_panelDropped l "panel dropped state" -STARTLINE +HIDDEN}
    addUserKnob {13 position_3d l Scale}
    position_3d {1 1 1}
    addUserKnob {13 position_3d_1 l Pivot}
    addUserKnob {7 hardness l Hardness}
    hardness 0.5
    addUserKnob {20 endGroup_1 l endGroup n -1}
    addUserKnob {26 ""}
    addUserKnob {26 text l "" +STARTLINE T "v1.0\nBy Geraud Mottais\nBased on the mtPrefStick by Miguel Torija\n"}
    }
    Input {
    inputs 0
    name Texture
    xpos -110
    ypos 154
    number 1
    }
    Crop {
    box {0 0 {width} {height}}
    name Crop2
    xpos -110
    ypos 187
    }
    Dot {
    name Dot11
    label Texture
    xpos -76
    ypos 262
    }
    set N1e3df170 [stack 0]
    Dot {
    name Dot8
    xpos -76
    ypos 885
    }
    Constant {
    inputs 0
    color {1 1 1 1}
    name Constant1
    xpos 97
    ypos -88
    }
    Input {
    inputs 0
    name Pref
    xpos 270
    ypos -296
    }
    Shuffle {
    in rgb
    name ShufflePref
    note_font "Verdana Bold"
    note_font_size 14
    xpos 270
    ypos -203
    }
    Switch {
    inputs 2
    which {{"!\[exists parent.input0]"}}
    name Switch3
    xpos 270
    ypos -65
    }
    Dot {
    name Dot4
    xpos 304
    ypos 82
    }
    set N313dd140 [stack 0]
    Dot {
    name Dot3
    xpos 304
    ypos 782
    }
    push $N313dd140
    ColorMatrix {
    matrix {
        {{parent.Axis2.matrix.0} {parent.Axis2.matrix.1} {parent.Axis2.matrix.2}}
        {{parent.Axis2.matrix.4} {parent.Axis2.matrix.5} {parent.Axis2.matrix.6}}
        {{parent.Axis2.matrix.8} {parent.Axis2.matrix.9} {parent.Axis2.matrix.10}}
      }
    name ColorMatrix1
    xpos 475
    ypos 78
    }
    Crop {
    box {0 0 {width} {height}}
    name Crop1
    xpos 475
    ypos 129
    }
    set N25c1ec60 [stack 0]
    Constant {
    inputs 0
    color {1 1 1 1}
    name Constant2
    xpos 1008
    ypos 33
    }
    Input {
    inputs 0
    name Beauty
    xpos 764
    ypos -129
    number 2
    }
    Crop {
    box {0 0 {width} {height}}
    name Crop3
    xpos 764
    ypos -96
    }
    Unpremult {
    channels all
    name Unpremult1
    xpos 764
    ypos -31
    }
    Switch {
    inputs 2
    which {{"!\[exists parent.input0]"}}
    name Switch2
    xpos 764
    ypos 56
    }
    Switch {
    inputs 2
    which {{parent.show}}
    name Switch4
    xpos 764
    ypos 129
    }
    Dot {
    name Dot6
    xpos 798
    ypos 262
    }
    set N1f1110d0 [stack 0]
    push $N1e3df170
    push $N25c1ec60
    BlinkScript {
    inputs 3
    recompileCount 261
    ProgramGroup 1
    KernelDescription "2 \"mtAdditiveKernel\" iterate pixelWise d6f19001e96dece487d3472145d33f13c675493cfd43a563a2f5a673349a7190 4 \"CG\" Read Point \"texture\" Read Random \"beautyTexture\" Read Random \"dst\" Write Random 24 \"aperture\" Float 1 AAAAAA== \"Pref Pick\" Float 4 AAAAAAAAAAAAAAAAAAAAAA== \"PrefPickmatte\" Float 4 AAAAAAAAAAAAAAAAAAAAAA== \"checkXY\" Bool 1 AA== \"checkXZ\" Bool 1 AA== \"checkYZ\" Bool 1 AA== \"checkProjection\" Bool 1 AA== \"invertx\" Bool 1 AA== \"inverty\" Bool 1 AA== \"scale\" Float 1 AACAPw== \"scaleX\" Float 1 AACAPw== \"scaleY\" Float 1 AACAPw== \"transformX\" Float 1 AAAAAA== \"transformY\" Float 1 AAAAAA== \"pmatteScale\" Float 4 AAAAPwAAAD8AAAA/AAAAPw== \"pmattePivot\" Float 4 AAAAAAAAAAAAAAAAAAAAAA== \"pmatteHardness\" Float 1 AAAAAA== \"PrefMatteMask\" Bool 1 AA== \"PrefMatteSPHERE\" Bool 1 AA== \"PrefMatteSquare\" Bool 1 AA== \"showMask\" Bool 1 AA== \"showPref\" Bool 1 AA== \"View UVs\" Bool 1 AA== \"camMatrix\" Float 16 AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA== 24 \"aperture\" 1 1 \"PrefPick\" 4 1 \"PrefPickmatte\" 4 1 \"checkXY\" 1 1 \"checkXZ\" 1 1 \"checkYZ\" 1 1 \"checkProjection\" 1 1 \"invertx\" 1 1 \"inverty\" 1 1 \"globalScale\" 1 1 \"scaleSTX\" 1 1 \"scaleSTY\" 1 1 \"transformSTX\" 1 1 \"transformSTY\" 1 1 \"pmatteScale\" 4 1 \"pmattePivot\" 4 1 \"pmatteHardness\" 1 1 \"PrefMatteMask\" 1 1 \"PrefMatteSPHERE\" 1 1 \"PrefMatteSquare\" 1 1 \"showMask\" 1 1 \"showPref\" 1 1 \"UnwrapUVs\" 1 1 \"camMatrix\" 16 1 15 \"textureconstEdgeColor\" Float 4 1 AAAAAAAAAAAAAAAAAAAAAA== \"beautyTextureconstEdgeColor\" Float 4 1 AAAAAAAAAAAAAAAAAAAAAA== \"prefNormalized\" Float 4 1 AAAAAAAAAAAAAAAAAAAAAA== \"prefNormalizedmatte\" Float 4 1 AAAAAAAAAAAAAAAAAAAAAA== \"PrefstMap\" Float 4 1 AAAAAAAAAAAAAAAAAAAAAA== \"transformedSTmap\" Float 4 1 AAAAAAAAAAAAAAAAAAAAAA== \"output\" Float 4 1 AAAAAAAAAAAAAAAAAAAAAA== \"outBeauty\" Float 4 1 AAAAAAAAAAAAAAAAAAAAAA== \"stMap\" Float 4 1 AAAAAAAAAAAAAAAAAAAAAA== \"UV\" Float 2 1 AAAAAAAAAAA= \"resolution\" Int 2 1 AAAAAAAAAAA= \"maskColor\" Float 4 1 AAAAAAAAAAAAAAAAAAAAAA== \"pmateValues\" Float 4 1 AAAAAAAAAAAAAAAAAAAAAA== \"pmatteMask\" Float 4 1 AAAAAAAAAAAAAAAAAAAAAA== \"pi\" Float 1 1 AAAAAA=="
    kernelSource "//This function is used to multiply a float3 by a matrix4x4\ninline float3 multVecMatrix( float3 src, float4x4 matrix)\n\{\n    float   x,y,z,w;\n\n    x = src\[0]*matrix\[0]\[0] + src\[1]*matrix\[1]\[0] +\n    src\[2]*matrix\[2]\[0] + matrix\[3]\[0];\n    y = src\[0]*matrix\[0]\[1] + src\[1]*matrix\[1]\[1] +\n    src\[2]*matrix\[2]\[1] + matrix\[3]\[1];\n    z = src\[0]*matrix\[0]\[2] + src\[1]*matrix\[1]\[2] +\n    src\[2]*matrix\[2]\[2] + matrix\[3]\[2];\n    w = src\[0]*matrix\[0]\[3] + src\[1]*matrix\[1]\[3] +\n    src\[2]*matrix\[2]\[3] + matrix\[3]\[3];\n    \n    return float3(x/w, y/w, z/w);\n\}\n\nkernel mtAdditiveKernel : ImageComputationKernel<ePixelWise>\n\{\n  Image<eRead, eAccessPoint, eEdgeClamped> CG;\n  Image<eRead, eAccessRandom, eEdgeConstant> texture;\n  Image<eRead, eAccessRandom, eEdgeConstant> beautyTexture;\n  Image<eWrite, eAccessRandom> dst; \n\n  param:\n    // This parameter is made available to the user.\n\n    float aperture;\n\n    float4 PrefPick;\n    float4 PrefPickmatte;\n    bool checkXY;\n    bool checkXZ;\n    bool checkYZ;\n    bool checkProjection; //Adding projection option\n    bool invertx;\n    bool inverty;\n    float globalScale;\n    float scaleSTX;\n    float scaleSTY;\n    float transformSTX;\n    float transformSTY;\n\n    float4 pmatteScale;\n    float4 pmattePivot;\n    float pmatteHardness;\n    bool PrefMatteMask;\n    bool PrefMatteSPHERE;\n    bool PrefMatteSquare;\n    bool showMask;\n    bool showPref;\n\n    bool UnwrapUVs;\n\n    float4x4 camMatrix;\n\n\n  local:\n    // This local variable is not exposed to the user.\n    float4 prefNormalized;\n    float4 prefNormalizedmatte;\n    float4 PrefstMap;\n    float4 transformedSTmap;\n    float4 output;\n    float4 outBeauty;\n    float4 stMap;\n    float2 UV;\n    int2 resolution;\n    float4 maskColor;\n\n    float4 pmateValues;\n    float4 pmatteMask;\n    float pi;\n\n  // In define(), parameters can be given labels and default values.\n  void define() \{\n    //defineParam(scale2ST, \"scaleOffset\", float2(1.00f));\n    defineParam(globalScale, \"scale\", 1.0f);\n    defineParam(scaleSTX, \"scaleX\", 1.0f);\n    defineParam(scaleSTY, \"scaleY\", 1.0f);\n    defineParam(transformSTX, \"transformX\", 0.0f); \n    defineParam(transformSTY, \"transformY\", 0.0f); \n    defineParam(PrefPick, \"Pref Pick\", float4(0.0f));\n    defineParam(pmatteScale, \"pmatteScale\", float4(0.5f));\n    defineParam(UnwrapUVs, \"View UVs\", false);\n\n  \}\n\n  // The init() function is run before any calls to process().\n  // Local variables can be initialized here.\n  void init() \{\n    resolution.x = CG.bounds.x2-CG.bounds.x1;\n    resolution.y = CG.bounds.y2-CG.bounds.y1;\n    //pi = 3.14159265359f;\n  \}\n\n  void process(int2 pos) \{\n\n    prefNormalized = CG()-(PrefPick);\n    if (fabs(prefNormalized.x)+fabs(prefNormalized.y)+fabs(prefNormalized.y) < 0.000001)\{\n      return;\n    \}\n    prefNormalizedmatte = CG()-(PrefPickmatte);\n\n    if (showPref and not UnwrapUVs)\{\n      dst(pos.x, pos.y) = prefNormalized;\n      return;\n    \}\n\n    pmateValues.x = (prefNormalizedmatte.x/pmatteScale.x)+pmattePivot.x;\n    pmateValues.y = (prefNormalizedmatte.y/pmatteScale.y)+pmattePivot.y;\n    pmateValues.z = (prefNormalizedmatte.z/pmatteScale.z)+pmattePivot.z;\n\n    // Sphere\n    if(PrefMatteSPHERE == 1)\{\n      pmatteMask = 1-(sqrt((pmateValues.x*pmateValues.x)+(pmateValues.y*pmateValues.y)+(pmateValues.z*pmateValues.z)));\n    \}\n    // Cube\n    if(PrefMatteSquare == 1)\{\n      pmatteMask = (max(fabs(pmateValues.x),fabs(pmateValues.y)));\n      pmatteMask = 1-(max(fabs(pmatteMask.x),fabs(pmateValues.z)));\n    \}\n\n    pmatteMask = pmatteMask/pmatteHardness;\n    pmatteMask = clamp(pmatteMask, float4(0.0f), float4(1.0f));\n\n    //if(checkProjection == 1)\{\n      //prefNormalized = float4(multVecMatrix(float3(prefNormalized.x, prefNormalized.y, prefNormalized.z), camMatrix),0);    \n    //\}\n    //dst(pos.x, pos.y) = prefNormalized;\n    //return; \n\n    if(checkXY == 1)\{\n      PrefstMap.x = ((prefNormalized.x*aperture)/resolution.x);\n      PrefstMap.y = ((prefNormalized.y*aperture)/resolution.y);\n      PrefstMap.z = 0.0f;\n    \}\n\n    else if(checkXZ == 1)\{\n      PrefstMap.x = ((prefNormalized.x*aperture)/resolution.x);\n      PrefstMap.y = ((prefNormalized.z*aperture)/resolution.y);\n      PrefstMap.z = 0.0f;\n    \}\n\n    else if(checkYZ == 1)\{\n      PrefstMap.x = ((prefNormalized.z*aperture)/resolution.x);\n      PrefstMap.y = ((prefNormalized.y*aperture)/resolution.y);\n      PrefstMap.z = 0.0f;\n    \}\n\n    transformedSTmap.x = ((PrefstMap.x/globalScale)/scaleSTX)+transformSTX;\n    transformedSTmap.y = ((PrefstMap.y/globalScale)/scaleSTY)+transformSTY;\n    \n    if(invertx == 1)\{\n    transformedSTmap.x = transformedSTmap.x*-1.0f;\n    \}\n\n    if(inverty == 1)\{\n    transformedSTmap.y = transformedSTmap.y*-1.0f;\n    \}    \n\n    stMap.x = transformedSTmap.x + 0.5f;\n    stMap.y = transformedSTmap.y + 0.5f;\n\n    UV.x = stMap.x*resolution.x-.5f;\n    UV.y = stMap.y*resolution.y-.5f;\n    output = bilinear(texture, UV.x, UV.y);\n\n    outBeauty = beautyTexture(pos.x, pos.y);\n    maskColor = float4(1,0,0,0);\n\n    if(showPref and UnwrapUVs)\{\n      outBeauty = (showMask)? prefNormalized*(float4(1.0f)-pmatteMask):prefNormalized;\n      maskColor = float4(0.3f,0.7f,1.0f,1.0f);\n    \}\n\n    if (UnwrapUVs)\{\n      //dst((int)UV.x, (int)UV.y) = (showMask)? outBeauty+maskColor*pmatteMask:outBeauty;\n      dst(pos.x, pos.y) = float4(stMap.x,stMap.y,0,pmatteMask.x);\n      //dst(((int)UV.x)+1, (int)UV.y) = (showMask)? (beautyTexture(pos.x+1, pos.y)+beautyTexture(pos.x-1, pos.y))/2+float4(1,0,0,0)*pmatteMask:(beautyTexture(pos.x+1, pos.y)+beautyTexture(pos.x-1, pos.y))/2;\n      return;\n      \n    \}\n\n    if(PrefMatteMask)\{\n      output = output * pmatteMask;\n    \}  \n\n    if (showMask)\{\n      output += float4(1,0,0,0)*pmatteMask;\n    \}\n    //output = output * maskAlpha;\n    // Write the result to the output image\n    dst(pos.x, pos.y) = output;\n  \}\n\};\n\n"
    useGPUIfAvailable {{parent.useGPUIfAvailable}}
    rebuild ""
    mtAdditiveKernel_aperture 36
    "mtAdditiveKernel_Pref Pick" {{parent.pref_pick} {parent.pref_pick} {parent.pref_pick} {parent.pref_pick}}
    mtAdditiveKernel_PrefPickmatte {{parent.matte_pick} {parent.matte_pick} {parent.matte_pick} {parent.matte_pick}}
    mtAdditiveKernel_checkXY {{directionPull==0?1:0}}
    mtAdditiveKernel_checkXZ {{directionPull==1?1:0}}
    mtAdditiveKernel_checkYZ {{directionPull==2?1:0}}
    mtAdditiveKernel_invertx {{parent.invert_u}}
    mtAdditiveKernel_inverty {{parent.invert_v}}
    mtAdditiveKernel_scale {{"parent.scale.w == parent.scale.h ? 1/parent.scale.w:1"}}
    mtAdditiveKernel_scaleX {{"parent.scale.w != parent.scale.h ? 1/parent.scale.w:1"}}
    mtAdditiveKernel_scaleY {{"parent.scale.w != parent.scale.h ? 1/parent.scale.h:1"}}
    mtAdditiveKernel_transformX {{parent.oX}}
    mtAdditiveKernel_transformY {{parent.oY}}
    mtAdditiveKernel_pmatteScale {{position_3d.x} {position_3d.y} {position_3d.z} 0}
    mtAdditiveKernel_pmattePivot {{parent.position_3d_1.x} {position_3d_1.y} {position_3d_1.z} 0}
    mtAdditiveKernel_pmatteHardness {{parent.hardness}}
    mtAdditiveKernel_PrefMatteMask {{parent.PrefMatteMask}}
    mtAdditiveKernel_PrefMatteSPHERE {{prefMatteForm==0?1:0}}
    mtAdditiveKernel_PrefMatteSquare {{prefMatteForm==1?1:0}}
    mtAdditiveKernel_showMask {{parent.show_mask}}
    mtAdditiveKernel_showPref {{parent.show}}
    "mtAdditiveKernel_View UVs" {{parent.ViewUnwrapedUVs}}
    mtAdditiveKernel_camMatrix {
        {{parent.parent.exrCamera_Read6.matrix} {parent.parent.exrCamera_Read6.matrix} {parent.parent.exrCamera_Read6.matrix} {parent.parent.exrCamera_Read6.matrix}}
        {{parent.parent.exrCamera_Read6.matrix} {parent.parent.exrCamera_Read6.matrix} {parent.parent.exrCamera_Read6.matrix} {parent.parent.exrCamera_Read6.matrix}}
        {{parent.parent.exrCamera_Read6.matrix} {parent.parent.exrCamera_Read6.matrix} {parent.parent.exrCamera_Read6.matrix} {parent.parent.exrCamera_Read6.matrix}}
        {{parent.parent.exrCamera_Read6.matrix} {parent.parent.exrCamera_Read6.matrix} {parent.parent.exrCamera_Read6.matrix} {parent.parent.exrCamera_Read6.matrix}}
      }
    rebuild_finalise ""
    name BlinkScript2
    xpos 475
    ypos 252
    }
    Dot {
    name Dot5
    xpos 509
    ypos 356
    }
    set N2b49d7d0 [stack 0]
    Dot {
    name Dot2
    xpos 509
    ypos 442
    }
    set N91d90e30 [stack 0]
    push $N2b49d7d0
    push $N1f1110d0
    Copy {
    inputs 2
    from0 rgba.alpha
    to0 mask.a
    name Copy1
    xpos 764
    ypos 346
    }
    C_STMap2_1 {
    inputs 2
    useGPUIfAvailable {{parent.useGPUIfAvailable}}
    uv rgb
    mode "warped src (inverse)"
    name C_STMap1
    xpos 764
    ypos 432
    }
    Dot {
    name Dot7
    xpos 798
    ypos 488
    }
    set N3b67e050 [stack 0]
    Dot {
    name Dot9
    xpos 670
    ypos 488
    }
    Dot {
    name Dot10
    xpos 670
    ypos 552
    }
    push $N3b67e050
    Grade {
    inputs 1+1
    white 0
    add {0.3 0.7 1 0}
    black_clamp false
    maskChannelMask mask.a
    name Grade1
    xpos 764
    ypos 548
    disable {{!parent.show_mask}}
    }
    Dot {
    name Dot1
    xpos 798
    ypos 733
    }
    push $N91d90e30
    Switch {
    inputs 2
    which {{ViewUnwrapedUVs}}
    name Switch1
    xpos 475
    ypos 729
    }
    CopyBBox {
    inputs 2
    name CopyBBox1
    xpos 475
    ypos 778
    disable {{ViewUnwrapedUVs}}
    }
    Merge2 {
    inputs 2
    bbox B
    mix {{parent.textureOpacity}}
    name Merge2
    label "\[if \{\[value useLifetime] \} \{return  \"\[value lifetimeStart]  - \[value lifetimeEnd]\"\} \{return\} ]"
    xpos 475
    ypos 881
    disable {{!ViewUnwrapedUVs}}
    }
    Output {
    name Output1
    xpos 475
    ypos 943
    }
    Axis2 {
    inputs 0
    rotate {{parent.rotate} {parent.rotate} {parent.rotate}}
    name Axis2
    xpos 485
    ypos -22
    }
    end_group
```
