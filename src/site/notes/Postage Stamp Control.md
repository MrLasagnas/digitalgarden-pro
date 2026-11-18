---
{"dg-publish":true,"permalink":"/postage-stamp-control/","tags":["nuke","node"]}
---



## 🧠 Description


## 🎯 When to use


## ⚠️ Known pitfalls

## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```
set cut_paste_input [stack 0]
version 17.1 v1
push $cut_paste_input
NoOp {
 name Postage_Stamps_Ctrl
 tile_color 0xbffcff
 label "- - - - - - - - - - "
 note_font "DejaVu Sans Bold"
 note_font_size 24
 selected true
 xpos -260
 ypos 44
 addUserKnob {20 User l buttons}
 addUserKnob {22 enable l ON T "for a in nuke.allNodes(\"Read\"):\n a\[\"postage_stamp\"].setValue(True)" +STARTLINE}
 addUserKnob {22 disable l OFF T "for a in nuke.allNodes(\"Read\"):\n a\[\"postage_stamp\"].setValue(False)" +STARTLINE}
}

```