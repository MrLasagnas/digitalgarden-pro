---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/quoicoufier/","tags":["nuke","node","fun"]}
---


# Quoicoufier

## 🧠 Description
To quoicoufy your script.
## 🎯 When to use
- When bored
- When trying to get fired

## ⚠️ Known pitfalls
BE SURE TO SAVE BEFORE USING

## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```nuke
    set cut_paste_input [stack 0]
    version 13.2 v8
    push $cut_paste_input
    NoOp {
    name Quoicoufier
    note_font "Comic Sans MS Bold"
    note_font_size 50
    selected true
    xpos -1146
    ypos -5768
    addUserKnob {20 User l "Banger Nodes"}
    addUserKnob {22 quoicounodes l Quoicoufier t "GET SOME QUOICOUNODES !!!" T "import nuke\n\ndef quoicounodes():\n    \n    nodes = nuke.allNodes()\n    \n    for node in nodes :\n        \n        nodename= node\[\"name\"].value()\n        node\[\"name\"].setValue(\"Quoicou\"+nodename)\n\n\nquoicounodes()" +STARTLINE}
    }
```
