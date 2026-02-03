---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/deep-soften/","tags":["nuke","node","deep"]}
---



## 🧠 Description


## 🎯 When to use

When you want to soften the z of a deepfromimage

## ⚠️ Known pitfalls

## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```
set cut_paste_input [stack 0]
version 13.0 v10
push $cut_paste_input
DeepExpression {
 temp_name0 smooth
 temp_expr0 "\[value softness]"
 chans1 deep
 rgba.alpha "rgba.alpha * 0.999999"
 deep.front "deep.front + offsetfront - smooth"
 deep.back "deep.back + offsetback + smooth"
 name DeepSoften
 selected true
 xpos -333
 ypos -10
 addUserKnob {20 User l DeepEdgeSmooth}
 addUserKnob {7 softness l Softness R 0 10}
 addUserKnob {7 offsetfront l "Offset Deep Front" R 0 10}
 addUserKnob {7 offsetback l "Offset Deep Back" R 0 10}
}

```