---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/na-n-fix/","tags":["nuke","node","QC"]}
---



## 🧠 Description

Fix NaN pixels by averaging with adjacent ones.
## 🎯 When to use

- During QC Phase
- After a [[Nuke_KB/10_Nodes/Rs Guided Blur\|Rs Guided Blur]] to get rid of NaN


## ⚠️ Known pitfalls

## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```
set cut_paste_input [stack 0]
version 13.0 v10
push $cut_paste_input
Expression {
 expr0 "isnan(r) ? ((r(x+1, y) + r(x+1, y+1) + r(x+1, y-1) + r(x, y+1) + r(x, y-1) + r(x-1, y+1) + r(x-1, y) + r(x-1, y-1)) / 8) : r"
 expr1 "isnan(g) ? ((g(x+1, y) + g(x+1, y+1) + g(x+1, y-1) + g(x, y+1) + g(x, y-1) + g(x-1, y+1) + g(x-1, y) + g(x-1, y-1)) / 8) : g"
 expr2 "isnan(b) ? ((b(x+1, y) + b(x+1, y+1) + b(x+1, y-1) + b(x, y+1) + b(x, y-1) + b(x-1, y+1) + b(x-1, y) + b(x-1, y-1)) / 8) : b"
 name NaN_Fix
 label "fixing Nan value pixel\nwith averaging"
 selected true
 xpos -1677
 ypos -214
}

```