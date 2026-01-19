---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/reformat2-bbox/","tags":["nuke","node","reformat","bbox"]}
---


# Reformat2Bbox

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
    Reformat {
    type "to box"
    box_width {{"max((max(bbox.r-width, -bbox.x)*2)*overscanpercent+width, width)"}}
    box_height {{"max((max(bbox.t-height, -bbox.y)*2)*overscanpercent+height, height)"}}
    box_fixed true
    resize none
    name Reformat2bbox
    selected true
    xpos -4406
    ypos -2212
    addUserKnob {20 User}
    addUserKnob {7 overscanpercent l "overscan multiply" R 0 2}
    overscanpercent 1
    }
```
