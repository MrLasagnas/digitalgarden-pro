---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/roto-merge/","tags":["nuke","node"]}
---

	

## 🧠 Description

A simple tool to merge received RGB rotoscope
## 🎯 When to use


## ⚠️ Known pitfalls

## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```
set cut_paste_input [stack 0]  
version 13.0 v10  
push $cut_paste_input  
Group {  
name RotoMerge9  
tile_color 0x666666ff  
label "\[if \{\[value _r]\} \{return \"<font color=red><b>R</font>\"\} else \{\}] \[if \{\[value _g]\} \{return \"<font color=green><b>G</font>\"\} else \{\}] \[if \{\[value _b]\} \{return \"<font color=blue><b>B</font>\"\} else \{\}] \[if \{\[value _a]\} \{return \"<font color=grey><b>A</font>\"\} else \{\}]"  
note_font_size 22  
selected true  
xpos -4240  
ypos -7579  
addUserKnob {20 User l Settings}  
addUserKnob {26 title l "" +STARTLINE T "<b><font size=3>RotoMerge</font></b><font size=1> v1.0</font><br>\n<font size=1>blur per channel</font>"}  
addUserKnob {26 ""}  
addUserKnob {26 channel l <b>Channels</b>}  
addUserKnob {6 _r l r -STARTLINE}  
_r true  
addUserKnob {6 _g l g -STARTLINE}  
_g true  
addUserKnob {6 _b l b -STARTLINE}  
_b true  
addUserKnob {6 _a l a -STARTLINE}  
_a true  
addUserKnob {26 size_4 l "<b>Blur size</b>"}  
addUserKnob {41 size l red T Blur1.size}  
addUserKnob {41 size_1 l green T Blur2.size}  
addUserKnob {41 size_2 l blue T Blur3.size}  
addUserKnob {41 size_5 l alpha T Blur4.size}  
addUserKnob {41 size_3 l all T Blur5.size}  
addUserKnob {26 ""}  
addUserKnob {26 _1 l "" +STARTLINE T oblanchet@weta}  
}  
Input {  
inputs 0  
name Input1  
xpos -741  
ypos 127  
}  
Blur {  
channels {rgba.red -rgba.green -rgba.blue none}  
name Blur1  
xpos -741  
ypos 167  
}  
Blur {  
channels {-rgba.red rgba.green -rgba.blue none}  
name Blur2  
xpos -741  
ypos 205  
}  
Blur {  
channels {-rgba.red -rgba.green rgba.blue none}  
name Blur3  
xpos -741  
ypos 243  
}  
Blur {  
channels alpha  
name Blur4  
xpos -741  
ypos 281  
}  
Expression {  
temp_name0 R  
temp_expr0 _r==1?r:0  
temp_name1 G  
temp_expr1 _g==1?g:0  
temp_name2 B  
temp_expr2 _b==1?b:0  
temp_name3 A  
temp_expr3 _a==1?a:0  
channel0 rgba  
expr0 max(R,G,B,A)  
name Expression1  
xpos -741  
ypos 319  
}  
Blur {  
channels rgba  
name Blur5  
xpos -741  
ypos 345  
}  
Output {  
name Output1  
xpos -741  
ypos 381  
}  
end_group
```