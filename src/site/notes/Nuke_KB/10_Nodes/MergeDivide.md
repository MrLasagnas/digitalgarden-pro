---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/merge-divide/","tags":["nuke","node","merge"]}
---


# MergeDivide

## 🧠 Description


## 🎯 When to use
- To be able to use a divide merge node and keep B pipe in order (for activating/deactivating)

## ⚠️ Known pitfalls


## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```nuke
    set cut_paste_input [stack 0]
    version 13.2 v8
    push $cut_paste_input
    MergeExpression {
     expr0 "Ar  <=0 ||  Br <=0   ?  0  :  Br/Ar"
     expr1 "Ag  <=0 ||  Bg <=0   ?  0  :  Bg/Ag"
     expr2 "Ab  <=0 ||  Bb <=0   ?  0  :  Bb/Ab"
     channel3 none
     expr3 "Aa  <=0 ||  Ba <=0   ?  0  :  Ba/Aa"
     name MergeDivide
     label B/A
     selected true
     xpos -4485
     ypos -2351
    }
```
