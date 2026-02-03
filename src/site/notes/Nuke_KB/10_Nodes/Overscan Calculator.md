---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/overscan-calculator/","tags":["nuke","node","reformat","overscan"]}
---



## 🧠 Description

Calculate the difference between bbox and format in pixel and %
## 🎯 When to use

- When you need to communicate the needed overscan to another departement

## ⚠️ Known pitfalls

## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```
set cut_paste_input [stack 0]
version 13.0 v10
push $cut_paste_input
NoOp {
 name Overscan_Calculator
 selected true
 xpos -777
 ypos 45
 addUserKnob {20 User}
 addUserKnob {15 shot_format l "shot format"}
 shot_format {0 0 {root.width} {root.height}}
 addUserKnob {15 bbox_input l "bbox input"}
 bbox_input {{bbox.x} {bbox.y} {bbox.r} {bbox.t}}
 addUserKnob {26 ""}
 addUserKnob {15 difference_pixel l "difference pixel"}
 difference_pixel {{"-(shot_format - bbox_input)"} {"-(shot_format - bbox_input)"} {"shot_format - bbox_input"} {"shot_format - bbox_input"}}
 addUserKnob {15 difference_percent l "difference percent"}
 difference_percent {{"(difference_pixel / root.width) * 100"} {"(difference_pixel / root.height) * 100"} {"(difference_pixel / root.width) * 100"} {"(difference_pixel / root.height) * 100"}}
 addUserKnob {26 ""}
 addUserKnob {15 difference_pixel_buffer l "difference pixel buffer"}
 difference_pixel_buffer {{"difference_pixel * 1.25"} {"difference_pixel * 1.25"} {"difference_pixel * 1.25"} {"difference_pixel * 1.25"}}
 addUserKnob {15 difference_percent_buffer l "difference percent buffer"}
 difference_percent_buffer {{"difference_percent * 1.25"} {"difference_percent * 1.25"} {"difference_percent * 1.25"} {"difference_percent * 1.25"}}
}

```