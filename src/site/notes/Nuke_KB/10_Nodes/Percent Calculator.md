---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/percent-calculator/","tags":["nuke","node","math"]}
---



## 🧠 Description

Small tool to rapidly calculate percentages on the go

## 🎯 When to use

- When you suck at maths


## ⚠️ Known pitfalls

## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```
set cut_paste_input [stack 0]
version 13.0 v10
push $cut_paste_input
NoOp {
 name PercentCalculator
 selected true
 xpos -18
 ypos -412
 addUserKnob {20 calculate l Calculate}
 addUserKnob {20 input_grp l INPUTS n 1}
 addUserKnob {26 div1 l "" +STARTLINE}
 addUserKnob {7 part l Part}
 part 200
 addUserKnob {7 total l Total}
 total 2000
 addUserKnob {7 percent l % R 0 100}
 percent 15
 addUserKnob {26 div2 l "" +STARTLINE}
 addUserKnob {20 endGroup n -1}
 addUserKnob {20 results l RESULTS n 1}
 addUserKnob {26 ""}
 addUserKnob {20 tab_grp l "" +STARTLINE n -2}
 addUserKnob {20 part_tab l Part}
 addUserKnob {7 part_result l "" +STARTLINE}
 part_result {{"(percent/100) * total"}}
 addUserKnob {20 total_tab l Total}
 addUserKnob {7 total_result l "" +STARTLINE}
 total_result {{"part * 100 / percent"}}
 addUserKnob {20 percent_tab l %}
 addUserKnob {7 percent_result l "" +STARTLINE R 0 100}
 percent_result {{"part / total * 100"}}
 addUserKnob {20 "" n -3}
 addUserKnob {26 ""}
}

```