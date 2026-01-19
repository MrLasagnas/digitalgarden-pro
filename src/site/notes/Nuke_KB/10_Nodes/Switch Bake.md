---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/switch-bake/","tags":["nuke","node","optimizing","organization"]}
---


# Switch Bake

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
    Switch {
    name Switch_Bake
    tile_color 0xff00ff
    label "\[if \{\[value knob.which]==1\} \{return \"Bake\"\} \{return \"Comp\"\}]\n\n\n\[if \{\[value knob.which]==1\} \{return \"\[knob this.tile_color 0xff000000]\"\} \{return \"\[knob this.tile_color 0xff00ff]\"\}] \n\n"
    note_font "Verdana Bold"
    selected true
    xpos -1123
    ypos -5978
    }
```
