---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/deflicker/","tags":["nuke","node"]}
---



## 🧠 Description


## 🎯 When to use


## ⚠️ Known pitfalls

## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```
Group {
 name DeFlicker
 help "Tool for deflickering footage or matching flicker from one footage to another. \n\nTo remove the flicker, connect the source input to the footage with flicker in it, then drag the region out over a flat area with flickering. Hit Analyse Region, select your reference frame, and then connect the source input to the destination footage. Finally, select Remove Flicker in the Mode drop-down menu. \n\nTo match the flicker, follow the same steps as above but instead select Match Flicker in the Mode drop-down menu. \n\nYou can also create a Dissolve node to blend between different lighting states. After following the steps above, hit the Normalise button, and then the Create Dissolve button. Tick the Link Output checkbox if you want to expression link the Dissolve node (for example if you create multiple ones), or untick it if you want to bake the keyframe animation. "
 tile_color 0x262626ff
 label "(\[value selectMode])"
 selected true
 xpos -117
 ypos -299
 addUserKnob {20 deflicker l DeFlicker}
 addUserKnob {41 go l "Analyse Region →" t "Analyse the selected region of the input. " T CurveTool_DeFlicker.go}
 addUserKnob {41 ROI l "" t "Select the region to analyse. " -STARTLINE T CurveTool_DeFlicker.ROI}
 addUserKnob {41 resetROI l Reset t "Reset the region to the current input format. " -STARTLINE T CurveTool_DeFlicker.resetROI}
 addUserKnob {26 ""}
 addUserKnob {4 selectMode l Mode t "Choose between matching and removing the flicker. " M {"Match Flicker" "Remove Flicker" "" ""}}
 selectMode "Remove Flicker"
 addUserKnob {3 refFrame l "Reference Frame" t "Select the reference frame. "}
 refFrame 1
 addUserKnob {22 setCurrentFrame l "Set to Current Frame" t "Set the reference frame to the current frame. " -STARTLINE T "import nuke\nnuke.thisNode()\['refFrame'].setValue(nuke.frame())"}
 addUserKnob {26 ""}
 addUserKnob {41 maskChannelMask l mask T MergeCopy.maskChannelMask}
 addUserKnob {41 invert_mask l invert -STARTLINE T MergeCopy.invert_mask}
 addUserKnob {41 unpremult l "(un)premult by" T Grade_DeFlicker.unpremult}
 addUserKnob {41 mix T MergeCopy.mix}
 addUserKnob {26 ""}
 addUserKnob {20 normaliseDissolveGroup l "Normalise → Dissolve" n 1}
 normaliseDissolveGroup 0
 addUserKnob {7 controller l INVISIBLE +INVISIBLE}
 addUserKnob {22 normalise l "→ Normalise" t "Normalise the resulting curve from the analysis, scaling the values to a range between 0 and 1 for use in a Dissolve node. " T "orig_knob = nuke.thisNode()\['controller']\n\nif nuke.thisNode()\['normValues'].isAnimated():\n    nuke.thisNode()\['normValues'].clearAnimated()\n    anim = None\n    orig_knob.clearAnimated()\n    orig_knob.setAnimated(True)\n\np = nuke.Panel('Enter Range')\np.addSingleLineInput('Start Frame' , '1')\np.addSingleLineInput('End Frame' , '100')\nres = p.show()\n\nstart = float(p.value('Start Frame'))\nend = float(p.value('End Frame'))\n\nfor fr in range(int(start), int(end)+1):\n    orig_knob.setExpression('(CurveTool_DeFlicker.intensitydata.r+CurveTool_DeFlicker.intensitydata.g+CurveTool_DeFlicker.intensitydata.b)/3')\n    orig_knob.animation(0).setKey(fr, orig_knob.animation(0).evaluate(fr))\n\nfor view in nuke.views():\n    # There's currently no way to ask a knob if it has an\n    # expression at a given view, so we have to check the\n    # AnimationCurve objects for that. However, we can still\n    # use knob.isAnimated() to partially optimize this.\n    if orig_knob.isAnimated(view=view):\n        aSize = 1 if orig_knob.singleValue(view) else orig_knob.arraySize()\n        for index in range(aSize):\n            anim = orig_knob.animation(index, view=view)\n            if anim is None or anim.noExpression():\n                continue\n            for f in range(int(start), int(end) + 1):\n                #knob.setValueAt(anim.evaluate(f), f, index)\n                anim.setKey(f, anim.evaluate(f))\n            orig_knob.setExpression('curve', channel=index, view=view)\n            # Even if the expression would have evaluated to a\n            # constant (flat) curve, we can't tell until after\n            # it has been baked and the expression is gone.\n            if anim.constant():\n                orig_knob.clearAnimated(index, view=view)\n\ntarget_knob = nuke.thisNode()\['normValues']\ntarget_knob.setAnimated()\nanim_list = orig_knob.animations()\n\nindex = 0\n\nfor a in anim_list:\n  anim = a.keys()\n  print anim\n\n  vals = list()\n  for val in anim:\n     vals.append(val.y)\n\n  high = max(vals)\n  low = min(vals)\n  diff = 1\n  low_val = 0\n\n  cvals = list()\n  for cval in vals:\n     cvals.append( cval - low )\n\n  chigh = max(cvals)\n  ratio = None\n  if (chigh > 0):\n     ratio = float(diff) / float(chigh)\n  else:\n     continue\n  print 'ratio is ' + str(ratio)\n  \n  for rvals in anim:\n     v = ( (rvals.y-low)*ratio ) + low_val\n     print v, rvals.x, index\n     target_knob.setValueAt( v , rvals.x)\n  index += 1\n" +STARTLINE}
 addUserKnob {7 normValues l INVISIBLE -STARTLINE +INVISIBLE}
 addUserKnob {22 createDissolve l "→ Create Dissolve" t "(Normalise first to create a curve for the Dissolve node to use).\n\nCreate a Dissolve node with a curve between 0 and 1 in the 'which' knob that is matching to the flicker. \n\nGreat for blending between two graded versions of what you are comping: one matching the darkest frame of the input (0) and one matching the brightest frame (1). \n\nChoose between a baked and expression linked Dissolve node using the Link Output checkbox. " -STARTLINE T "a = nuke.thisNode()\nb = nuke.thisNode().name()\nc = nuke.thisNode().knob('linkOutput').getValue()\n\ndef flickerDissolve():\n\td = nuke.createNode('Dissolve', inpanel = False)\n\td\['label'].setValue('Normalised Flicker')\n\td\['which'].fromScript(a\['normValues'].toScript())\n\ndef flickerDissolveExp():\n\td = nuke.createNode('Dissolve', inpanel = False)\n\td\['label'].setValue('Normalised Flicker')\n\td\['which'].setExpression('\{\}.normValues'.format(b))\n\nnuke.root().begin()\n\nif c == 1:\n\tflickerDissolveExp()\nelse:\n\tflickerDissolve()\n"}
 addUserKnob {6 linkOutput l "Link Output" t "Choose between a baked and expression linked Dissolve node. " -STARTLINE}
 linkOutput true
 addUserKnob {20 endGroup n -1}
 addUserKnob {26 info l "" +STARTLINE}
 addUserKnob {26 creator l "" +STARTLINE T "Kenn Hedin Kalvik"}
 addUserKnob {26 version l "" +STARTLINE T "DeFlicker v1.1 | 2022"}
 addUserKnob {26 website l "" +STARTLINE T "<a href='https://www.keheka.com/'>www.keheka.com</a>"}
}
 Input {
  inputs 0
  name mask
  xpos -35
  ypos -25
  number 1
 }
 Dot {
  name Dot6
  xpos -1
  ypos 63
 }
 Input {
  inputs 0
  name source
  xpos -180
  ypos -352
 }
 Dot {
  name Dot2
  xpos -146
  ypos -204
 }
set N6fd9f400 [stack 0]
 Dot {
  name Dot4
  xpos -277
  ypos -204
 }
set N6fd9f000 [stack 0]
 Grade {
  white {{"\[\n\nif \{\[numvalue CurveTool_DeFlicker.intensitydata.r] == 0\} \{return 1\} else \{return CurveTool_DeFlicker.intensitydata.r/CurveTool_DeFlicker.intensitydata.r(refFrame)\} \n\n]\n\n\n\n"} {"\[\n\nif \{\[numvalue CurveTool_DeFlicker.intensitydata.g] == 0\} \{return 1\} else \{return CurveTool_DeFlicker.intensitydata.g/CurveTool_DeFlicker.intensitydata.g(refFrame)\} \n\n]\n\n\n\n"} {"\[\n\nif \{\[numvalue CurveTool_DeFlicker.intensitydata.b] == 0\} \{return 1\} else \{return CurveTool_DeFlicker.intensitydata.b/CurveTool_DeFlicker.intensitydata.b(refFrame)\} \n\n]\n\n\n\n"} 1}
  reverse {{parent.selectMode}}
  black_clamp false
  name Grade_DeFlicker
  xpos -311
  ypos -23
 }
 Dot {
  name Dot1
  xpos -277
  ypos 63
 }
push $N6fd9f400
 Merge2 {
  inputs 2+1
  operation copy
  name MergeCopy
  xpos -180
  ypos 59
 }
 Output {
  name Output1
  xpos -180
  ypos 234
 }
push $N6fd9f000
 Dot {
  name Dot3
  xpos -405
  ypos -204
 }
 CurveTool {
  avgframes 1
  ROI {480 270 1440 810}
  intensitydata {0 0 0 0}
  name CurveTool_DeFlicker
  xpos -439
  ypos -101
 }
end_group
```