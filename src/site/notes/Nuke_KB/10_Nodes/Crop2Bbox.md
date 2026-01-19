---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/crop2-bbox/","tags":["nuke","node","bbox","reformat"]}
---


# Crop2Bbox

## 🧠 Description
Crop2bbox

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
    name croptobbox1
    selected true
    xpos -1088
    ypos -6004
    }
    Input {
    inputs 0
    name Input1
    xpos 462
    ypos -609
    }
    Crop {
    box {{bbox.x} {bbox.y} {bbox.r} {bbox.t}}
    reformat true
    crop false
    name Crop320
    xpos 462
    ypos -562
    }
    Output {
    name Output1
    xpos 462
    ypos -469
    }
    end_group
```
