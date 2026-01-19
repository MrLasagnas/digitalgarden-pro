---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/st-map-generator/","tags":["nuke","node","pass","transform"]}
---


# STMap Generator

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
    Expression {
    expr0 (x+0.5)/width
    expr1 (y+0.5)/height
    name STMAP_Gradient
    selected true
    xpos -4330
    ypos -2713
    }
```
