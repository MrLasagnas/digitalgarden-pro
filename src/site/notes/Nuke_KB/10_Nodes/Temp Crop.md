---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/temp-crop/","tags":["nuke","node","reformat","optimizing","organization"]}
---


# Temp Crop

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
    Crop {
    box {93 1888 1498 2454}
    reformat true
    crop false
    name Master_Crop2
    selected true
    xpos 180
    ypos -3166
    }
    Reformat {
    resize none
    name Reformat17
    selected true
    xpos 180
    ypos -3090
    }
    Transform {
    translate {{-(root.format.width-(MasterCropValues.x+MasterCropValues.r))/2 x1019 -54} {-(root.format.height-(MasterCropValues.y+MasterCropValues.t))/2}}
    center {1024 768}
    filter Lanczos4
    name Repo_After_Crop3
    selected true
    xpos 180
    ypos -3044
    addUserKnob {20 User}
    addUserKnob {15 MasterCropValues}
    MasterCropValues {{parent.Master_Crop2.box} {parent.Master_Crop2.box} {parent.Master_Crop2.box} {parent.Master_Crop2.box}}
    }
```
