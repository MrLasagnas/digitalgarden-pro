---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/pointsto3d/","tags":["nuke","node","3D","tracking"]}
---


# Pointsto3d

## 🧠 Description
Tracks points to generate them in 3D space

## 🎯 When to use


## ⚠️ Known pitfalls


## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```nuke
    set cut_paste_input [stack 0]
    version 13.0 v10
    push $cut_paste_input
    PointsTo3D {
    name PointsTo3D6
    selected true
    xpos -32875
    ypos 16676
    }
```
