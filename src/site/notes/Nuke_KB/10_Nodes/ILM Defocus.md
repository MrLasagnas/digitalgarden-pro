---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/ilm-defocus/","tags":["nuke","node"]}
---



## 🧠 Description


## 🎯 When to use


## ⚠️ Known pitfalls

## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```
set cut_paste_input [stack 0]  
version 13.0 v10  
BackdropNode {  
inputs 0  
name BackdropNode14  
tile_color 0xaaaaaa00  
label "23A-2a Lens 55 mm. aperture 2.8 \nget the information from shotgrid\ncheck the slate number and go to the slate page to find the aperture\nhttps://[yard.shotgunstudio.com/page/54773](http://yard.shotgunstudio.com/page/54773)"  
note_font_size 20  
selected true  
xpos -1208  
ypos 11599  
bdwidth 616  
bdheight 291  
}  
push $cut_paste_input  
Dot {  
name Dot78  
selected true  
xpos -1355  
ypos 11534  
}  
set N309cc860 [stack 0]  
Dot {  
name Dot79  
selected true  
xpos -1617  
ypos 11534  
}  
Group {  
inputs 0  
name ILM_bokeh1  
tile_color 0x46689bff  
label "---\n\[value bokeh]"  
selected true  
xpos -1826  
ypos 11615  
postage_stamp true  
addUserKnob {20 Bokeh}  
addUserKnob {4 bokeh l "bokeh shape" M {backlight "big spots" blades blurry clean confusion grainy messy moon obscured octogon orb petagon plain silk spots spotty wave wood "" ""}}  
bokeh octogon  
addUserKnob {6 previewAll l "preview all" +STARTLINE}  
addUserKnob {6 viewLabels l "view labels" -STARTLINE}  
viewLabels true  
addUserKnob {26 ""}  
addUserKnob {12 translate}  
addUserKnob {26 ""}  
addUserKnob {7 rotate R -180 180}  
addUserKnob {14 scale R 0 100}  
scale 1  
addUserKnob {26 ""}  
addUserKnob {7 skewX l "skew X" R -1 1}  
addUserKnob {7 skewY l "skew Y" R -1 1}  
addUserKnob {20 Version}  
addUserKnob {26 nodeInfo l "" +STARTLINE T "bokeh - v1.0\n\n'Bokeh' is a simple tool for loading in different bokeh shapes to run through your defocus setup.\n"}  
addUserKnob {26 ""}  
addUserKnob {26 versionInfo l "" +STARTLINE T "Version Release: 1.0\n\nAuthors: Enrik Pavdeja (epavdeja) \n\nLast Edited By: Enrik Pavdeja (epavdeja)\n\nRelease Date: 11/06/2016"}  
}  
Constant {  
inputs 0  
color {0 0 0 1}  
name Constant1  
xpos -3120  
ypos 495  
}  
Crop {  
box {0 0 256 29}  
reformat true  
name Crop3  
xpos -3120  
ypos 614  
}  
Dot {  
name Dot1  
xpos -3086  
ypos 666  
}  
set Nd817efa0 [stack 0]  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Wave.png"  
format "119 119 0 0 119 119 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read52  
xpos -700  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat22  
xpos -700  
ypos 608  
}  
set N4b1216f0 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge22  
xpos -700  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 87 97 118 101 46 112 110 103}  
  }  
old_expression_markers {{0 12}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text21  
xpos -700  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid41  
xpos -700  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Wood.png"  
format "229 229 0 0 229 229 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read53  
xpos -590  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat23  
xpos -590  
ypos 608  
}  
set N92e00d0 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge23  
xpos -590  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 87 111 111 100 46 112 110 103}  
  }  
old_expression_markers {{0 12}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text22  
xpos -590  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid42  
xpos -590  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Spotty.png"  
format "195 195 0 0 195 195 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read51  
xpos -810  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat21  
xpos -810  
ypos 608  
}  
set Nb9aa6c0 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge21  
xpos -810  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 83 112 111 116 116 121 46 112 110 103}  
  }  
old_expression_markers {{0 14}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 14 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97 9 97 10 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text20  
xpos -810  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid40  
xpos -810  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Spots.png"  
format "288 288 0 0 288 288 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read50  
xpos -920  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat20  
xpos -920  
ypos 608  
}  
set N4b61fa90 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge20  
xpos -920  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 83 112 111 116 115 46 112 110 103}  
  }  
old_expression_markers {{0 13}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97 9 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text19  
xpos -920  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid39  
xpos -920  
ypos 902  
}  
push $Nd817efa0  
Reformat {  
inputs 0  
format "256 256 0 0 256 256 1 square_256"  
name Reformat19  
xpos -1030  
ypos 608  
}  
set N94232e0 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge19  
xpos -1030  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{91 102 105 108 101 32 116 97 105 108 32 91 107 110 111 98 32 110 111 100 101 55 102 57 53 50 50 49 102 57 57 100 48 46 102 105 108 101 93 32 93}  
  }  
old_expression_markers {{17 32}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 14 22 15 22 16 22 17 22 18 22 19 22 20 22 21 22 22 22 23 22 24 22 25 22 26 22 27 22 28 22 29 22 30 22 31 22 32 22 33 22 34 22 35 22 36 22 37 22 38 22 39 22 40 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text18  
xpos -1030  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid38  
xpos -1030  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Silk.png"  
format "266 266 0 0 266 266 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read47  
selected true  
xpos -1140  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat18  
xpos -1140  
ypos 608  
}  
set N14aef4e0 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge18  
xpos -1140  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 83 105 108 107 46 112 110 103}  
  }  
old_expression_markers {{0 12}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text17  
xpos -1140  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid37  
xpos -1140  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Plain.png"  
format "233 233 0 0 233 233 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read45  
xpos -1250  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat17  
xpos -1250  
ypos 608  
}  
set N795693c0 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge17  
xpos -1250  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 80 108 97 105 110 46 112 110 103}  
  }  
old_expression_markers {{0 13}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97 9 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text16  
xpos -1250  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid36  
xpos -1250  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Petagon.png"  
format "318 318 0 0 318 318 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read44  
xpos -1360  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat16  
xpos -1360  
ypos 608  
}  
set N7a6bd100 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge16  
xpos -1360  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 80 101 116 97 103 111 110 46 112 110 103}  
  }  
old_expression_markers {{0 15}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 14 22 15 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97 9 97 10 97 11 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text15  
xpos -1360  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid35  
xpos -1360  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Orb.png"  
format "180 180 0 0 180 180 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read43  
xpos -1470  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat15  
xpos -1470  
ypos 608  
}  
set Nc0a87f0 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge15  
xpos -1470  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 79 114 98 46 112 110 103}  
  }  
old_expression_markers {{0 11}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text14  
xpos -1470  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid34  
xpos -1470  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Octogon.png"  
format "266 266 0 0 266 266 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read42  
xpos -1580  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat14  
xpos -1580  
ypos 608  
}  
set N7a1ad8e0 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge14  
xpos -1580  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 79 99 116 111 103 111 110 46 112 110 103}  
  }  
old_expression_markers {{0 15}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 14 22 15 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97 9 97 10 97 11 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text13  
xpos -1580  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid33  
xpos -1580  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Obscured.png"  
format "434 434 0 0 434 434 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read41  
xpos -1690  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat13  
xpos -1690  
ypos 608  
}  
set Na4577650 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge13  
xpos -1690  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 79 98 115 99 117 114 101 100 46 112 110 103}  
  }  
old_expression_markers {{0 16}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 14 22 15 22 16 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97 9 97 10 97 11 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text12  
xpos -1690  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid32  
xpos -1690  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Moon.png"  
format "269 269 0 0 269 269 1 "  
origset true  
on_error black  
version 2  
name Read40  
xpos -1800  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat12  
xpos -1800  
ypos 608  
}  
set N8eb6b50 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge12  
xpos -1800  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 77 111 111 110 46 112 110 103}  
  }  
old_expression_markers {{0 12}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text11  
xpos -1800  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid31  
xpos -1800  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Messy.png"  
format "270 270 0 0 270 270 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read39  
xpos -1910  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat11  
xpos -1910  
ypos 608  
}  
set N45a2fc20 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge11  
xpos -1910  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 77 101 115 115 121 46 112 110 103}  
  }  
old_expression_markers {{0 13}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97 9 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text10  
xpos -1910  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid30  
xpos -1910  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Grainy.png"  
format "313 313 0 0 313 313 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read37  
xpos -2020  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat10  
xpos -2020  
ypos 608  
}  
set Nbfd1c30 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge10  
xpos -2020  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 71 114 97 105 110 121 46 112 110 103}  
  }  
old_expression_markers {{0 14}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 14 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97 9 97 10 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text9  
xpos -2020  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid29  
xpos -2020  
ypos 902  
}  
push $Nd817efa0  
Reformat {  
inputs 0  
format "256 256 0 0 256 256 1 square_256"  
name Reformat9  
xpos -2130  
ypos 608  
}  
set Nc011c50 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge9  
xpos -2130  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{91 102 105 108 101 32 116 97 105 108 32 91 107 110 111 98 32 110 111 100 101 55 102 57 52 102 56 52 102 101 52 56 48 46 102 105 108 101 93 32 93}  
  }  
old_expression_markers {{17 32}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 14 22 15 22 16 22 17 22 18 22 19 22 20 22 21 22 22 22 23 22 24 22 25 22 26 22 27 22 28 22 29 22 30 22 31 22 32 22 33 22 34 22 35 22 36 22 37 22 38 22 39 22 40 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text8  
xpos -2130  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid28  
xpos -2130  
ypos 902  
}  
push $Nd817efa0  
Reformat {  
inputs 0  
format "256 256 0 0 256 256 1 square_256"  
name Reformat8  
xpos -2240  
ypos 608  
}  
set Nc13a4a0 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge8  
xpos -2240  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{91 102 105 108 101 32 116 97 105 108 32 91 107 110 111 98 32 110 111 100 101 55 102 57 53 48 55 102 98 49 54 49 48 46 102 105 108 101 93 32 93}  
  }  
old_expression_markers {{17 32}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 14 22 15 22 16 22 17 22 18 22 19 22 20 22 21 22 22 22 23 22 24 22 25 22 26 22 27 22 28 22 29 22 30 22 31 22 32 22 33 22 34 22 35 22 36 22 37 22 38 22 39 22 40 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text7  
xpos -2240  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid27  
xpos -2240  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Confusion.png"  
format "175 175 0 0 175 175 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read34  
xpos -2350  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat7  
xpos -2350  
ypos 608  
}  
set Ne0156fc0 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge7  
xpos -2350  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 67 111 110 102 117 115 105 111 110 46 112 110 103}  
  }  
old_expression_markers {{0 17}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 14 22 15 22 16 22 17 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97 9 97 10 97 11 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text6  
xpos -2350  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid26  
xpos -2350  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Clean.png"  
format "110 110 0 0 110 110 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read33  
xpos -2460  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat6  
xpos -2460  
ypos 608  
}  
set Nb7762690 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge6  
xpos -2460  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 67 108 101 97 110 46 112 110 103}  
  }  
old_expression_markers {{0 13}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97 9 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text5  
xpos -2460  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid25  
xpos -2460  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Blurry.png"  
format "409 409 0 0 409 409 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read31  
xpos -2570  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat5  
xpos -2570  
ypos 608  
}  
set N23100320 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge5  
xpos -2570  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 66 108 117 114 114 121 46 112 110 103}  
  }  
old_expression_markers {{0 14}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 14 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97 9 97 10 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text4  
xpos -2570  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid24  
xpos -2570  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Blades.png"  
format "125 125 0 0 125 125 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read30  
xpos -2680  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat4  
xpos -2680  
ypos 608  
}  
set Nb041fb90 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge4  
xpos -2680  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 66 108 97 100 101 115 46 112 110 103}  
  }  
old_expression_markers {{0 14}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 14 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97 9 97 10 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text3  
xpos -2680  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid23  
xpos -2680  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Big-Spots.png"  
format "204 209 0 0 204 209 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read29  
xpos -2790  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat3  
xpos -2790  
ypos 608  
}  
set N8878e910 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge3  
xpos -2790  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 66 105 103 45 83 112 111 116 115 46 112 110 103}  
  }  
old_expression_markers {{0 17}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 14 22 15 22 16 22 17 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97 9 97 10 97 11 97}  
  }  
cursor_position 34  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text2  
xpos -2790  
ypos 854  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid22  
xpos -2790  
ypos 902  
}  
push $Nd817efa0  
Read {  
inputs 0  
file_type png  
file "/mnt/prod/aha/references/ILM/20260206_ILM_petzval_nuke_script/_nuke/comp/optical_flares/Optical Flares Textures/Elements/Iris_Backlight.png"  
format "101 101 0 0 101 101 1 "  
origset true  
on_error black  
version 2  
in_colorspace scene_linear  
out_colorspace scene_linear  
name Read28  
xpos -2900  
ypos 489  
addUserKnob {20 User}  
addUserKnob {26 _atlas_skip_deepscan -STARTLINE +HIDDEN T file}  
}  
Reformat {  
format "256 256 0 0 256 256 1 square_256"  
name Reformat2  
xpos -2900  
ypos 608  
}  
set N485541a0 [stack 0]  
Merge2 {  
inputs 2  
bbox B  
mix 0.8  
name Merge1  
xpos -2900  
ypos 806  
disable {{!viewLabels}}  
}  
Text2 {  
font_size_toolbar 22  
font_width_toolbar 100  
font_height_toolbar 100  
message "\[file tail \[knob \[topnode].file] ]"  
old_message {{73 114 105 115 95 66 97 99 107 108 105 103 104 116 46 112 110 103}  
  }  
old_expression_markers {{0 17}  
  }  
box {10 0 256 30}  
yjustify center  
transforms {{0 2}  
  }  
font_size_values {{0 22 1 22 2 22 3 22 4 22 5 22 6 22 7 22 8 22 9 22 10 22 11 22 12 22 13 22 14 22 15 22 16 22 17 22 0 97 1 97 2 97 3 97 4 97 5 97 6 97 7 97 8 97 9 97 10 97 11 97}  
  }  
font {{ Liberation Sans : Regular : liberation-sans/LiberationSans-Regular.ttf : 0 }}  
font_size 22  
scale {1 1}  
cursor_initialised true  
autofit_bbox false  
initial_cursor_position {{0 540}  
  }  
group_animations {{0} imported: 0 selected: items: "root transform/"}  
animation_layers {{1 11 480 270 0 0 1 1 0 0 0 0}  
  }  
color {0.856667 0.846667 0.036667 1}  
color_panelDropped true  
name Text477  
xpos -2900  
ypos 858  
disable {{!viewLabels}}  
}  
Grid {  
number 1  
color {0.653333 0.653333 -0.346667 1}  
color_panelDropped true  
name Grid225  
xpos -2900  
ypos 902  
}  
ContactSheet {  
inputs 22  
width 1536  
height 1024  
rows 4  
columns 6  
gap 12  
center true  
roworder TopBottom  
name ContactSheet1  
xpos -2900  
ypos 1094  
}  
Dot {  
name Dot2  
xpos -2866  
ypos 1242  
}  
push $N92e00d0  
push $N4b1216f0  
push $Nb9aa6c0  
push $N4b61fa90  
push $N94232e0  
push $N14aef4e0  
push $N795693c0  
push $N7a6bd100  
push $Nc0a87f0  
push $N7a1ad8e0  
push $Na4577650  
push $N8eb6b50  
push $N45a2fc20  
push $Nbfd1c30  
push $Nc011c50  
push $Nc13a4a0  
push $Ne0156fc0  
push $Nb7762690  
push $N23100320  
push $Nb041fb90  
push $N8878e910  
push $N485541a0  
Switch {  
inputs 22  
which {{parent.bokeh}}  
name Switch1  
xpos -260  
ypos 758  
}  
Transform {  
translate {{parent.translate} {parent.translate}}  
rotate {{parent.rotate}}  
scale {{parent.scale} {parent.scale}}  
skewX {{parent.skewX}}  
skewY {{parent.skewY}}  
center {128 128}  
shutteroffset centred  
name Transform  
xpos -260  
ypos 855  
}  
Crop {  
box {0 0 256 256}  
name Crop1  
xpos -260  
ypos 902  
}  
Shuffle {  
green red  
blue red  
alpha red  
name Shuffle1  
label "\[value in]-->\[value out]"  
xpos -260  
ypos 944  
}  
Switch {  
inputs 2  
which {{parent.previewAll}}  
name Switch2  
xpos -260  
ypos 1238  
}  
Output {  
name Output1  
xpos -260  
ypos 1430  
}  
end_group  
Group {  
inputs 2  
name ILM_Kernel  
knobChanged "import nuke\nimport json\n\ndef preset_knobChanged(thisNode=None, thisKnob=None):\n    thisNode = thisNode or nuke.thisNode()\n    thisKnob = thisKnob or nuke.thisKnob()\n    if thisKnob.name() == 'presets':\n        if not thisNode\['_data_presets'].value():\n            nuke.tprint('No Data Stored on node')\n            return\n        data_dict = json.loads(thisNode\['_data_presets'].value())\n        if thisKnob.value() in list(data_dict):\n            knobs_presets = data_dict.get(thisKnob.value())\n            for knob in list(knobs_presets):\n                if knob in list(nuke.thisNode().knobs().keys()):\n                    thisNode\[knob].fromScript(knobs_presets.get(knob))\n                    \ndef update_division_nodes(thisNode):\n    dependent_expr_nodes = thisNode.dependent(nuke.EXPRESSIONS)\n    for node in dependent_expr_nodes:\n        try:\n            type_value = node.knob('type').value()\n        except:\n            nuke.message(\"Something wrong with this setup!\")\n            return\n        if type_value == 'sb.divisions':\n            rebuild_TimeWarps(node)\n\ndef rebuild_TimeWarps(parentNode):\n    BLACK_LIST = \['Input', 'Output']\n    parentNode.begin()\n    tws = nuke.toNode('TimeWarps')\n    this = nuke.thisNode()\n    method = this.knob('method').value()\n    cell_x, cell_y = this.knob('cellDivisions').value()\n    tws.begin()\n    for node in nuke.allNodes():\n        if [node.name](http://node.name)() not in BLACK_LIST:\n            nuke.delete(node)\n    root = nuke.toNode(\"Input\")\n    if method == \"TimeBlur\":\n        return\n    splits = int(cell_x* cell_y)\n    all_nodes = \[]\n    for i in range(splits):\n        value  = (1/(splits))*(i)%(splits)+(1/(splits)/2)\n        tw = nuke.nodes.TimeWarp(inputs =\[root], lookup = 'frame+%s' % str(value) , filter = 0, length = 0)\n        all_nodes.append(tw)\n    all_nodes.insert(2, None)\n    merge = nuke.nodes.Merge2(inputs = all_nodes, operation = \"over\", also_merge = 'all')\n    output = nuke.toNode('Output') \n    output.setInput(0, merge)\n    tws.end()\n    parentNode.end()\n\npreset_knobChanged()\nRED_COLOR = 0xFF000000\nNO_COLOR = 0x242e57ff #SwirlyBokehPalette\nthisNode = nuke.thisNode()\nthisKnob = nuke.thisKnob()\nif thisKnob.name() == \"Switch2\":\n    depentend_nodes = thisNode.dependent(nuke.EXPRESSIONS)\n    if thisKnob.value():\n        for n in depentend_nodes:\n            n\['tile_color'].setValue(NO_COLOR)\n    else:\n        for n in depentend_nodes:\n            n\['tile_color'].setValue(RED_COLOR)\n\nelif thisKnob.name() == \"Switch\":\n    if thisKnob.value():\n        thisNode\[\"Edit_Kernel\"].setVisible(False)\n    else:\n        thisNode\[\"Edit_Kernel\"].setVisible(True)\n        \nelif thisKnob.name() == \"cellDivisions\" or thisKnob.name() == \"method\" or thisKnob.name() == \"presets\":\n    update_division_nodes(thisNode)\n"  
tile_color 0x242e57ff  
label "Swirly Bokeh"  
note_font_size 21  
note_font_color 0xffb300ff  
selected true  
xpos -1651  
ypos 11634  
addUserKnob {20 Control}  
addUserKnob {20 setup l Set-Up n 1}  
addUserKnob {4 presets l Presets M {"" Default}}  
presets Default  
addUserKnob {22 save_preset l "Save Preset" T "\nimport json\n\nexclusion_knobs = \['help', 'dope_sheet', 'hide_input', 'note_font_color', 'onCreate', 'updateUI', 'rootNodeUpdated', 'knobChanged', 'note_font', 'tile_color', 'bookmark', 'selected', 'autolabel', 'window', 'label', 'onDestroy', 'lock_connections', 'indicators', 'panel', 'xpos', 'lifetimeEnd', 'ypos', 'postage_stamp_frame', 'postage_stamp', 'disable', 'lifetimeStart', 'icon', 'name', 'cached', 'mapsize', 'note_font_size', 'useLifetime', 'gl_color', 'gsv']\n\ncustom_knobs = \['presets', '_data_presets', 'save_preset', 'delete_preset', 'Infos', 'text_3', 'processing_layers', 'box', 'string', 'IGNORE_LAYERS', 'removeDIMattes', 'DIMattes', 'lut', 'text', 'text_1', 'python_button', 'python_button_1','python_button_2']\n\nexclusion_knobs += custom_knobs\n\ndef save_presets(thisNode=None):\n    global exclusion_knobs\n    thisNode = thisNode or nuke.thisNode()\n\n    if thisNode\['_data_presets'].value():\n        data_dict = json.loads(thisNode\['_data_presets'].value())\n    else:\n        data_dict = \{\}\n\n    panel = nuke.Panel('Save Presets')\n    panel.addEnumerationPulldown('preset_update', '\\n'.join(\['__New__'] + \[x.replace(' ','@') for x in list(data_dict)]))\n    panel.addSingleLineInput('preset_name','')\n    \n    if not panel.show():\n        return\n    if panel.value('preset_update') == '__New__' and not panel.value('preset_name'):\n        nuke.message('Please write the name of the preset')\n        return\n\n    if panel.value('preset_name') not in \[x.replace(' ','@') for x in list(data_dict)]:\n        data_dict\[panel.value('preset_name')] = \{\}\n\n    if panel.value('preset_update')  != '__New__':\n        prest_name = panel.value('preset_update')\n    else:\n        prest_name = panel.value('preset_name')\n   \n    for knob in thisNode.allKnobs():\n        if [knob.name](http://knob.name)() not in exclusion_knobs and knob.Class() != 'Tab_Knob' and [knob.name](http://knob.name)():\n            data_dict\[prest_name]\[[knob.name](http://knob.name)()] = knob.toScript()\n\n    thisNode\['_data_presets'].setValue(json.dumps(data_dict))\n    thisNode\['presets'].setValues(list(data_dict))\n    thisNode\['presets'].setValue(prest_name)\n\n\nsave_presets()\n" +STARTLINE}  
addUserKnob {22 delete_preset l "Delete Preset" -STARTLINE T "\nimport json \ndef remove_preset(thisNode=None):\n    thisNode =  thisNode or nuke.thisNode()\n    if thisNode\['_data_presets'].value():\n        data_dict = json.loads(thisNode\['_data_presets'].value())\n    else:\n        return\n    \n    panel = nuke.Panel('Remove Presets')\n    panel.addEnumerationPulldown('preset_name', '\\n'.join(\[x.replace(' ','@') for x in list(data_dict.keys())]))\n    \n    if not panel.show():\n        return\n    \n    del data_dict\[panel.value('preset_name').replace('@',' ')]\n\n    thisNode\['_data_presets'].setValue(json.dumps(data_dict))\n    thisNode\['presets'].setValues(list(data_dict.keys()))\n    thisNode\['presets'].setValue(0)\n\n\nremove_preset()\n"}  
addUserKnob {1 _data_presets +INVISIBLE}  
_data_presets "\{\"\": \{\}, \"Default\": \{\"group_view_position\": \"top-left\", \"show_group_view\": \"false\", \"Switch\": \"false\", \"text_4\": \"\", \"PAR\": \"1\", \"Size\": \"1.4\", \"corners\": \"11\", \"EdgeFlattening\": \"1.3\", \"corners_angle\": \"38\", \"skew_switch\": \"false\", \"skewing\": \"7\", \"esmooth\": \"44\", \"gain\": \"0.52\", \"dust\": \"0.14\", \"dust_seed\": \"14\", \"dust_size\": \"32\", \"gamma\": \"1\", \"spechlg\": \"-1\", \"spec_seed\": \"190\", \"spechlg_scale\": \"10\", \"ripple_switch\": \"true\", \"ripples\": \"0.37\", \"ripple_amount\": \"0.88\", \"ripple_blur\": \"4.95\", \"noise_breakup\": \"7.5\", \"noise_amp\": \"70\", \"OnionRing\": \"1\", \"Ringsize\": \"0.7\", \"mix\": \"1\", \"size\": \"-0.6\", \"size_1\": \"3.4\", \"InsideRingBlur\": \"4\", \"Ringblur\": \"4\", \"EdgeBlur\": \"7\", \"enable_angle\": \"false\", \"angle_1\": \"52\", \"erode_size\": \"30\", \"falloff\": \"30\", \"enableca\": \"false\", \"rgb\": \"rgba.red -rgba.green -rgba.blue rgba.alpha\", \"ca\": \"1.25\", \"cascale\": \"true\", \"xbias\": \"1.1\", \"ybias\": \"1\", \"Switch2\": \"true\", \"text_5\": \"\", \"text_6\": \"\", \"cellDivisions\": \"10 5\", \"method\": \"TimeWarps\", \"catseyeswitch\": \"true\", \"catseyeW\": \"0.3\", \"catseyeH\": \"0.15\", \"catseyeCentreW\": \"0\", \"catseyeCentreH\": \"-0.4\", \"catseyeSoftness\": \"0\", \"barndoorswitch\": \"false\", \"barnDoorW\": \"0\", \"barnDoorH\": \"0\", \"barnDoorCentreW\": \"0\", \"barnDoorCentreH\": \"0\", \"barndoorSoftness\": \"0\", \"selfoccluswitch\": \"false\", \"filterSliceW\": \"0.3\", \"filterSliceH\": \"0.15\", \"filterSliceCentreW\": \"0\", \"filterSliceCentreH\": \"-0.4\", \"blurocclu\": \"0\", \"apbladewitch\": \"false\", \"corners_1\": \"8\", \"apBladeW\": \"0\", \"apBladeH\": \"0\", \"apBladeCenterW\": \"0\", \"apBladeCenterH\": \"0\", \"corner_rotation\": \"40\", \"edgeflatting\": \"0\", \"blurbladeocclu\": \"0\", \"tearswitch\": \"false\", \"teardrop\": \"0\", \"pointiness\": \"1\", \"squash\": \"0\", \"stretch\": \"0\", \"softness_1\": \"120\", \"text_12\": \"\", \"text_7\": \"\", \"filter\": \"none\", \"manualc\": \"false\", \"ccatseye_switch_x\": \"true\", \"ccatseye_switch_y\": \"true\", \"ccatseye_switch_w\": \"true\", \"ccatseye_switch_h\": \"true\", \"ccatseyex\": \"0\", \"ccatseyey\": \"0\", \"ccatseyew\": \"0\", \"ccatseyeh\": \"0\", \"text_1_1\": \"\", \"cbarndoor_switch_x\": \"true\", \"cbarndoor_switch_y\": \"true\", \"cbarndoor_switch_w\": \"true\", \"cbarndoor_switch_h\": \"true\", \"cbarndoorx\": \"0\", \"cbarndoory\": \"0\", \"cbarndoorw\": \"0\", \"cbarndoorh\": \"0\", \"text_2\": \"\", \"cselfocclusion_switch_x\": \"true\", \"capblade_switch_y\": \"true\", \"cselfocclusion_switch_w\": \"true\", \"cselfocclusion_switch_h\": \"true\", \"cselfocclux\": \"0\", \"cselfoccluy\": \"0\", \"cselfoccluw\": \"0\", \"cselfoccluh\": \"0\", \"text_3_1\": \"\", \"capblade_switch_x_1\": \"true\", \"capblade_switch_y_1\": \"true\", \"capblade_switch_w\": \"true\", \"capblade_switch_h\": \"true\", \"capbladex\": \"0\", \"capbladey\": \"0\", \"capbladew\": \"0\", \"capbladeh\": \"0\", \"text_9\": \"\", \"scalecompensation\": \"1\", \"type\": \"\", \"text_8\": \"\"\}\}"  
addUserKnob {20 endGroup_13 l endGroup n -1}  
addUserKnob {26 ""}  
addUserKnob {6 Switch l "<b><font color=#97e1f1> Use input kernel</font><b>" +STARTLINE}  
Switch true  
addUserKnob {26 text_4 l "            " T "<i>Disable to display the Kernel Editor properties</i>   "}  
addUserKnob {20 Edit_Kernel l "Edit Built in Kernel " t "Adjust Pixel aspect ratio, oinion ring, inside pattern... \n" +HIDDEN n 1}  
addUserKnob {7 PAR l "Pixel aspect ratio" R 1 2}  
PAR 1.5  
addUserKnob {7 Size t "Tweaking the size will affect the aspect of the bokeh so make sure the kernel fits in the 256px square" R 1 2}  
Size 1.25  
addUserKnob {41 corners l Blades t "Aperture number of blades (corners).\n11 blades will usually give you the most circular bokeh. Tweak the edge flattening below to smooth the corners a bit." T Flare1.corners}  
addUserKnob {7 EdgeFlattening l "Edge flattening" t "Will smooth the blade edges.\n1 = straight edge = default value" R 0.5 1.5}  
EdgeFlattening 1.13  
addUserKnob {41 corners_angle l Angle t "Kernel angle" T Flare1.corners_angle}  
addUserKnob {6 skew_switch l "Enable Skew (Cooke effect)" t "Turn on or off the Cooke lens effect, works best when the edge flattening is inferior to 1 with a blade number of 8 or below." +STARTLINE}  
addUserKnob {20 Skewing l "Skewing settings" n 1}  
Skewing 0  
addUserKnob {7 skewing l Skewing t "Helps to mimic the Cooke lenses effect when the kernel has 8 blades" R -15 15}  
skewing 5.5  
addUserKnob {7 esmooth l "Edge smoothing" R 1 60}  
esmooth 35.8  
addUserKnob {20 endGroup_3 n -1}  
addUserKnob {26 ""}  
addUserKnob {7 gain l Gain R -1 2}  
gain 0.27  
addUserKnob {7 dust l Dust t "Fills the Kernel with a bit of noise to mimic a dirt on the rear element" R -1 1}  
dust 0.03  
addUserKnob {3 dust_seed l Seed -STARTLINE}  
dust_seed 16  
addUserKnob {7 dust_size l "Dust scale" R 1 100}  
dust_size 32  
addUserKnob {41 gamma l "Inner gamma" T Grade2.gamma}  
addUserKnob {7 spechlg l "Specular highlights" R -1 1}  
spechlg -0.6  
addUserKnob {3 spec_seed l Seed -STARTLINE}  
spec_seed 16  
addUserKnob {7 spechlg_scale l "Specular scale" R 1 50}  
spechlg_scale 20  
addUserKnob {26 ""}  
addUserKnob {6 ripple_switch l "Enable Ripples" +STARTLINE}  
ripple_switch true  
addUserKnob {20 Ripples l "Ripples settings" n 1}  
Ripples 0  
addUserKnob {7 ripples l Ripples t "Add a fingerprint pattern"}  
ripples 0.185  
addUserKnob {7 ripple_amount l Scale R 0 3}  
ripple_amount 0.88  
addUserKnob {7 ripple_blur l Blur R 0 5}  
ripple_blur 4.95  
addUserKnob {7 noise_breakup l "Noise breakup" R 0 10}  
noise_breakup 7.5  
addUserKnob {7 noise_amp l Amplitude -STARTLINE R 0 70}  
noise_amp 70  
addUserKnob {20 endGroup_2 n -1}  
addUserKnob {26 ""}  
addUserKnob {7 OnionRing l "Onion ring gain" t "Adds a glow edge around the Kernel" R -1 2}  
OnionRing 1  
addUserKnob {7 Ringsize l "Ring size" R 1 4}  
Ringsize 0.05  
addUserKnob {41 mix l "Ring shadow" T Grade3.mix}  
addUserKnob {41 size l "Shadow size" T Erode4.size}  
addUserKnob {41 size_1 l "Shadow blur" -STARTLINE T Blur19.size}  
addUserKnob {7 InsideRingBlur l "Inside ring blur" t "Blur the inside edge of the kernel" R 0 10}  
InsideRingBlur 10  
addUserKnob {7 Ringblur l "Outside ring blur" t "Adds a touch of blur around the edge of the Kernel." -STARTLINE R 0 10}  
Ringblur 10  
addUserKnob {7 EdgeBlur l "Edge diffusion" t "Overall edge blur" R 0 10}  
EdgeBlur 10  
addUserKnob {26 ""}  
addUserKnob {6 enable_angle l Angle t "Turns on edge from angle control" +STARTLINE}  
enable_angle true  
addUserKnob {20 angle_settings l "Angle settings" n 1}  
angle_settings 0  
addUserKnob {7 angle_1 l Angle R 0 360}  
angle_1 238  
addUserKnob {7 erode_size l Size R -50 50}  
erode_size 2  
addUserKnob {7 falloff l Falloff R 0 60}  
falloff 34.8  
addUserKnob {20 endGroup_4 l endGroup n -1}  
addUserKnob {26 ""}  
addUserKnob {6 enableca l "Enable Chromatic Aberrations" +STARTLINE}  
enableca true  
addUserKnob {41 rgb l channels T GodRays1.channels}  
addUserKnob {7 ca l Amount R -1 1}  
ca 1.25  
addUserKnob {6 cascale l Scale -STARTLINE}  
cascale true  
addUserKnob {41 xbias l "X bias" t "Gives a slight X offset on the scaling." T GodRays1.xbias}  
addUserKnob {41 ybias l "Y bias" t "Gives a slight Y offset on the scaling" T GodRays1.ybias}  
addUserKnob {20 endGroup n -1}  
addUserKnob {26 ""}  
addUserKnob {6 Switch2 l "<b><font color=#97e1f1> Enable SwirlyBokeh</font><b>" t "Display option.\nDisable the splits to display the built in or input Kernel.\nMust be turned off when you are done." +STARTLINE}  
Switch2 true  
addUserKnob {26 text_5 l "            " T "<i>When disabled, the 2 other groups turn red = inactive </i> \n\n\n\n\n\n"}  
addUserKnob {26 text_6 l "   " T "       "}  
addUserKnob {30 cellDivisions l Splits t "Changes the scale of the crop box. U defines the width and V defines the height. The higher the number is the smaller the box will be which will increase the swirlling precision."}  
cellDivisions {8 4}  
addUserKnob {83 method l Method t "Timewarps have been implemented in v2,4, it should offer more stability." M {TimeBlur TimeWarps}}  
method TimeWarps  
addUserKnob {26 ""}  
addUserKnob {6 catseyeswitch l "Enable Cats Eye" +STARTLINE}  
catseyeswitch true  
addUserKnob {41 catseyeW l "X Offset" T NoOp2.catseyeW}  
addUserKnob {41 catseyeH l "Y Offset" -STARTLINE T NoOp2.catseyeH}  
addUserKnob {41 catseyeCentreW l "Scale W" T NoOp2.catseyeCentreW}  
addUserKnob {41 catseyeCentreH l "Scale H" -STARTLINE T NoOp2.catseyeCentreH}  
addUserKnob {41 catseyeSoftness l Softness T NoOp2.catseyeSoftness}  
addUserKnob {26 ""}  
addUserKnob {6 barndoorswitch l "Enable Barn Door" +STARTLINE}  
addUserKnob {41 barnDoorW l "X Offset" T NoOp2.barnDoorW}  
addUserKnob {41 barnDoorH l "Y Offset" -STARTLINE T NoOp2.barnDoorH}  
addUserKnob {41 barnDoorCentreW l "Scale W" T NoOp2.barnDoorCentreW}  
addUserKnob {41 barnDoorCentreH l "Scale H" -STARTLINE T NoOp2.barnDoorCentreH}  
addUserKnob {41 barndoorSoftness l Softness T NoOp2.barndoorSoftness}  
addUserKnob {26 ""}  
addUserKnob {6 selfoccluswitch l "Enable SelfOcclusion" +STARTLINE}  
addUserKnob {41 filterSliceW l "X Offset" T NoOp2.filterSliceW}  
addUserKnob {41 filterSliceH l "Y Offset" -STARTLINE T NoOp2.filterSliceH}  
addUserKnob {41 filterSliceCentreW l "Scale W" T NoOp2.filterSliceCentreW}  
addUserKnob {41 filterSliceCentreH l "Scale H" -STARTLINE T NoOp2.filterSliceCentreH}  
addUserKnob {7 blurocclu l Softness t "Adds blur to the occlusion matte so the edge isn't so sharp" R 0 10}  
addUserKnob {26 ""}  
addUserKnob {6 apbladewitch l "Enable Polygonal Occlusion" +STARTLINE}  
addUserKnob {41 corners_1 l Count -STARTLINE T Flare2.corners}  
addUserKnob {41 apBladeW l "X Offset" T NoOp2.apBladeW}  
addUserKnob {41 apBladeH l "Y Offset" -STARTLINE T NoOp2.apBladeH}  
addUserKnob {41 apBladeCenterW l "Scale W" T NoOp2.apBladeCenterW}  
addUserKnob {41 apBladeCenterH l "Scale H" -STARTLINE T NoOp2.apBladeCenterH}  
addUserKnob {41 corner_rotation l Rotation T Flare2.corner_rotation}  
addUserKnob {41 edgeflatting l "Edge Flattening" -STARTLINE T Flare2.edgeflatting}  
addUserKnob {7 blurbladeocclu l Softness R 0 5}  
addUserKnob {26 ""}  
addUserKnob {6 tearswitch l "Enable Tearing, Squash and Stretch" +STARTLINE}  
addUserKnob {41 teardrop T TeardropCTRL.teardrop}  
addUserKnob {41 pointiness T TeardropCTRL.pointiness}  
addUserKnob {41 squash T TeardropCTRL.squash}  
addUserKnob {41 stretch T TeardropCTRL.stretch}  
addUserKnob {26 ""}  
addUserKnob {26 ""}  
addUserKnob {41 box +INVISIBLE T Crop2.box}  
addUserKnob {41 softness_1 l "Softness / Section blend" t "Blends the sections together with a smoother edge.\nFor low scale images and bbox please use a low value such as 40 to 80.\nCan go higher if the input image is larger, like 120 or more." T Crop2.softness_1}  
addUserKnob {26 text_12 l " " T "Small input image = 40 to 80\nLarge input image = 120 or more"}  
addUserKnob {20 advanced l Advanced}  
addUserKnob {26 text l "<b>What is that?</b>" T "The advanced parameters give addtional control on how\nyou want to shape the bokeh from center to corners. \nYou can contrain the bokeh in the center from getting the\nswirly treatment by either selecting one of the filters or by \ntweaking things through the manual constraint mode."}  
addUserKnob {26 ""}  
addUserKnob {26 text_7 l " " T "Constrain the center occlusion to 0 but \npreserve the treatment on the sides."}  
addUserKnob {41 filter l "Filter type" T NoOp2.filter}  
addUserKnob {26 ""}  
addUserKnob {41 manualc l "Manual constraint (overwrite smoothness filter)" T NoOp2.manualc}  
addUserKnob {20 group_constraint l "Occlusion modes" n 1}  
addUserKnob {26 text_1 l "" +STARTLINE T Catseye}  
addUserKnob {41 ccatseye_switch_x l "X Offset" T NoOp2.ccatseye_switch_x}  
addUserKnob {41 ccatseye_switch_y l "Y Offset" -STARTLINE T NoOp2.ccatseye_switch_y}  
addUserKnob {41 ccatseye_switch_w l "Scale W" -STARTLINE T NoOp2.ccatseye_switch_w}  
addUserKnob {41 ccatseye_switch_h l "Scale H" -STARTLINE T NoOp2.ccatseye_switch_h}  
addUserKnob {41 ccatseyex l "X Offset" T NoOp2.ccatseyex}  
addUserKnob {41 ccatseyey l "Y Offset" -STARTLINE T NoOp2.ccatseyey}  
addUserKnob {41 ccatseyew l "Scale W" T NoOp2.ccatseyew}  
addUserKnob {41 ccatseyeh l "Scale H" -STARTLINE T NoOp2.ccatseyeh}  
addUserKnob {26 ""}  
addUserKnob {26 text_1_1 l "" +STARTLINE T Barndoor}  
addUserKnob {41 cbarndoor_switch_x l "X Offset" T NoOp2.cbarndoor_switch_x}  
addUserKnob {41 cbarndoor_switch_y l "Y Offset" -STARTLINE T NoOp2.cbarndoor_switch_y}  
addUserKnob {41 cbarndoor_switch_w l "Scale W" -STARTLINE T NoOp2.cbarndoor_switch_w}  
addUserKnob {41 cbarndoor_switch_h l "Scale H" -STARTLINE T NoOp2.cbarndoor_switch_h}  
addUserKnob {41 cbarndoorx l "X Offset" T NoOp2.cbarndoorx}  
addUserKnob {41 cbarndoory l "Y Offset" -STARTLINE T NoOp2.cbarndoory}  
addUserKnob {41 cbarndoorw l "Scale W" T NoOp2.cbarndoorw}  
addUserKnob {41 cbarndoorh l "Scale H" -STARTLINE T NoOp2.cbarndoorh}  
addUserKnob {26 ""}  
addUserKnob {26 text_2 l "" +STARTLINE T SelfOcclusion}  
addUserKnob {41 cselfocclusion_switch_x l "X Offset" T NoOp2.cselfocclusion_switch_x}  
addUserKnob {41 capblade_switch_y l "Y Offset" -STARTLINE T NoOp2.capblade_switch_y}  
addUserKnob {41 cselfocclusion_switch_w l "Scale W" -STARTLINE T NoOp2.cselfocclusion_switch_w}  
addUserKnob {41 cselfocclusion_switch_h l "Scale H" -STARTLINE T NoOp2.cselfocclusion_switch_h}  
addUserKnob {41 cselfocclux l "X Offset" T NoOp2.cselfocclux}  
addUserKnob {41 cselfoccluy l "Y Offset" -STARTLINE T NoOp2.cselfoccluy}  
addUserKnob {41 cselfoccluw l "Scale W" T NoOp2.cselfoccluw}  
addUserKnob {41 cselfoccluh l "Scale H" -STARTLINE T NoOp2.cselfoccluh}  
addUserKnob {26 ""}  
addUserKnob {26 text_3_1 l "" +STARTLINE T "Aperture Blade"}  
addUserKnob {41 capblade_switch_x_1 l "X Offset" T NoOp2.capblade_switch_x}  
addUserKnob {41 capblade_switch_y_1 l "Y Offset" -STARTLINE T NoOp2.capblade_switch_y}  
addUserKnob {41 capblade_switch_w l "Scale W" -STARTLINE T NoOp2.capblade_switch_w}  
addUserKnob {41 capblade_switch_h l "Scale H" -STARTLINE T NoOp2.capblade_switch_h}  
addUserKnob {41 capbladex l "X Offset" T NoOp2.capbladex}  
addUserKnob {41 capbladey l "Y Offset" -STARTLINE T NoOp2.capbladey}  
addUserKnob {41 capbladew l "Scale W" T NoOp2.capbladew}  
addUserKnob {41 capbladeh l "Scale H" -STARTLINE T NoOp2.capbladeh}  
addUserKnob {20 endGroup_1 l endGroup n -1}  
addUserKnob {26 ""}  
addUserKnob {26 text_9 l " " T "Field curvature compensation :\nGently use this slider if the side of frame kernel gets too small."}  
addUserKnob {41 scalecompensation l "Scale Compensation" T Transform_SizeCompensation.scalecompensation}  
addUserKnob {26 type -STARTLINE +INVISIBLE T sb.kernel}  
addUserKnob {20 DIMattes_1 l DiMattes}  
addUserKnob {26 text_8 l "<b>DI Mattes<b>" t "When using a kernel as input filter in a zdefocus node, you can chose to either have one or multiple channels. This is super handy if your kernel has a specific rgb look (chromatic aberrations or else)\nWhen having multple channels, the zDefocus needs some help to get any extra layers - other than rgba - to be defocused. This feature allows you do automaticaly create shuffle nodes that will stream all the layers you need so they get properly defocused keeping the same aspect as the rgba output." T "If \"use input channels\" is checked in your defocus tool, \nuse this DI Mattes feature to defocus any extra layers \nother than rgba.\nThe defocus tool must have \"all channels\" selected.\n"}  
addUserKnob {22 DIMattes l "Process DI Mattes" t "Hit the \"Process DI Mattes\" button to get all the mattes computed through SwirlyBokeh." T "this = nuke.thisNode()\n\nCONNECT_NODE_DICT = \{'ZDefocusLens_v03':2\}\n\ns = nuke.toNode('Image')\nstartNode = nuke.toNode('DIMATTE')\n\nignore_layers = this\['IGNORE_LAYERS'].value()\nignore_layers = ignore_layers.replace(\" \",\"\")\nIGNORE_LAYERS = ignore_layers.split(\",\")\n\nDELETE_NODES = \[\"Dot\", \"Shuffle\"]\n\nallLayers = nuke.layers(s)\nallLayers = \[layer for layer in allLayers if layer not in IGNORE_LAYERS]\n\nnextNodes = startNode.dependent(nuke.INPUTS)\n\nshuffleDetected = True\n\nwhile shuffleDetected:\n    nextNodes = startNode.dependent(nuke.INPUTS)\n    for node in nextNodes:\n        if node.Class() in DELETE_NODES:\n            nuke.delete(node)\n        else:\n            shuffleDetected = False\n\nconnectNode = startNode\n\nfor layer in allLayers:\n    s = nuke.nodes.Shuffle()#(\"Shuffle\", inpanel=False)\n    s.setInput(0, connectNode)\n    s\['out'].setValue(layer)\n    label =  \"\{\}-->\{\}\".format( s\['in'].value(), s\['out'].value() )\n    s\['label'].setValue(label)\n    connectNode = s\n\nif allLayers != \[]:\n    this\['processing_layers'].setValue(\", \".join(allLayers))\nelse:\n    this\['processing_layers'].setValue('none')\n\n\nfor node in nextNodes:\n    try: \n        inputNumber = CONNECT_NODE_DICT\[node.Class()]\n    except KeyError:\n        inputNumber = 0\n    node.setInput(inputNumber, connectNode)\n\n" +STARTLINE}  
addUserKnob {22 removeDIMattes l "Remove DI Mattes" t "Do as it says." -STARTLINE T "this = nuke.thisNode()\n\nstartNode = nuke.toNode('DIMATTE')\n\nCONNECT_NODE_DICT = \{'ZDefocusLens_v03':2\}\n\nDELETE_NODES = \[\"Dot\", \"Shuffle\"]\n\nnextNodes = startNode.dependent(nuke.INPUTS)\n\nshuffleDetected = True\nconnectNode = startNode\n\nwhile shuffleDetected:\n    nextNodes = startNode.dependent(nuke.INPUTS)\n    for node in nextNodes:\n        if node.Class() in DELETE_NODES:\n            nuke.delete(node)\n        else:\n            shuffleDetected = False\n\nfor node in nextNodes:\n    try: \n        inputNumber = CONNECT_NODE_DICT\[node.Class()]\n    except KeyError:\n        inputNumber = 0\n    node.setInput(inputNumber, connectNode)\n\nthis\['processing_layers'].setValue('none')"}  
addUserKnob {1 IGNORE_LAYERS l "Ignore Layers:" t "Here you can write any layers that are already being defocused (RGBA) or that don't need to be defocused (tech layers = depth,motion,pref...)"}  
IGNORE_LAYERS rgb,rgba,alpha,depth  
addUserKnob {26 processing_layers l "Processed Layers:" t "Displays all the dimatte layers that will be defocused." T none}  
addUserKnob {26 "" l " "}  
addUserKnob {20 ReadMe}  
addUserKnob {26 Infos l "" +STARTLINE T "Swirly Bokeh important infos :\n\nThe setup can be used with ANY defocus tool as long as you have the option to plug a \nfilter.\nIt is highly recommended to do precomps before and after using this setup. \nMake sure you precomp all channels. (rgba+depth+dimattes)\nSwirlyCrop node is there to cancel all animated curves before we use the SwirlyBokeh. \nIt also helps to optimize the defocus calculation. \nThe 2nd part (TimeBlur) of the setup uses the 1st part (Kernel) curves to get the job done.\n\nStep by step guide :\n1. Use the built in Kernel to create or match the plate Bokeh. \nYou can also use an input \"Custom' Kernel as an option.\nOnce the number of blades, aspect ratio and overall look is set,  you can check the box\n \"Enable Swirly Bokeh\".\n\n2. Splits dertimine how many time the image is getting cropped.\nA high value will give you a more accurate result BUT it will take longer to compute.\n\nEx : If you put 5, the image will be splitted 25 times and the bokeh transition from \ncenter to corners will get smoother.\nIf you set the value to 10, it will split your image 100 times and will need a lot of \nressorces.\n\n3. Scale occlusion is affecting the output bokeh look. It will give the bokeh the catseye \naspect.\nThe more lemon shaped the kernel looks the more catseyes effect you will get.\n\n4. Transform occlusion gives you the control to adjust the shape of the Kernel on \nX or Y axis. \n\n5. Soften will blur and blend the transition between the splits (animated crops).\n\n6. Original Gizmo from Chris Fryer, more info here :\nhttps://[www.chrisfryer.co.uk/post/cfcatseyedefocus\nBig](http://www.chrisfryer.co.uk/post/cfcatseyedefocus%5CnBig) big thanks to you Chris!\n\n"}  
addUserKnob {1 string l "Confluence Page"}  
string [https://confluence.lucasfilm.com/display/COMP/Swirly+Bokeh](https://confluence.lucasfilm.com/display/COMP/Swirly+Bokeh)  
addUserKnob {26 text_3 l " Updates log :" T "06/2025 - Timewarp hot fix to bypass errors\n                  Profile importer fix by Vit\n03/2025 - Timewarp method added for better stability by @vitmusatov\n                   Added more controls to edit the kernel\n01/2025 - Profile manager now supports Nuke15\n                  Reworked advanced tab entirely\n                  to give more control \n                  Added new aperture blade occlusion mode\n                  Revisted tool layout to take less space in the properties tab\n                  Optimization tweaks \n11/2024 - Added checkbox for each occlusion\n                   methods and fixed minor issues\n08/2024 - Fixed stability issue caused by bbox management and\n                   a knob error\n                   Adjusted chromab center expression \n                   and added X and Y sliders  for more control\t\n                   Kernel alpha channel is now driven by a lumakey\n04/2024 - Enabled all modes for better control over the occlusion\n                   Fixed bug where 1 section would stay black\n                   Fixed PAR expression that was messing up the ripples\n                   New alpha formula for the Kernel\n03/2024 - Onion ring does no longer appear on the softened edge \n01/2024 - Added dimattes manager\n                   Added curve profiles\n                   Improved kernel display\n                   Improved outer edge and noise control\n                   New chromatic aberration feature              \n11/2023 - Fixed slicedMatte in the Kernel group\n11/2023 - New formula and features from zDefocusCells by Chris Fryer\n10/2023 - Added barn door option\n09/2023 - Added ripple and cooke lense options\n\nSwirlybokeh v2.4 \n@Florian Sanchez 2022 "}  
}  
BackdropNode {  
inputs 0  
name BackdropNode1  
tile_color 0xaaaaaa00  
label "zDefocusCells formula"  
note_font_size 42  
xpos -2123  
ypos -1833  
bdwidth 467  
bdheight 364  
}  
StickyNote {  
inputs 0  
name StickyNote2  
tile_color 0xdb5a08ff  
label Dust  
note_font_size 12  
note_font_color 0xffffffff  
xpos -364  
ypos 166  
}  
Reformat {  
inputs 0  
format "255 255 0 0 255 255 1 square 255"  
name Reformat3  
xpos -861  
ypos -414  
}  
Expression {  
temp_name0 center_x  
temp_expr0 Ripple_Settings.center.x  
temp_name1 center_y  
temp_expr1 Ripple_Settings.center.y  
temp_name2 size  
temp_expr2 Ripple_Settings.size  
temp_name3 par  
temp_expr3 PAR  
channel0 rgba  
expr0 "sin(hypot((x-center_x)*par, y-center_y)/size)"  
channel1 none  
channel2 none  
name Ripple_Expression  
xpos -861  
ypos -362  
}  
Blur {  
channels rgba  
size {{ripple_blur}}  
name Blur8  
xpos -861  
ypos -305  
}  
Group {  
name jittermizer_v08_1_GRP  
help "randomizes the position of each pixel within a certain distance\nUse for adding noise. Works like Randomise in shake or Dither in CompTime.\nUse filter type presets for different types of noise. You will get large noise or finer noise.\nSet amplitude above 0 for this to work"  
knobChanged "\nn = nuke.thisNode()\nk = nuke.thisKnob()\nif [k.name](http://k.name)() in \[\"showPanel\", \"JitterPattern\"]:\n    n\['size'].setVisible(n.knob(\"JitterPattern\").value() == \"Custom\")\n    n\['irregularity'].setVisible(n.knob(\"JitterPattern\").value() == \"Custom\")\n    n\['intensity'].setVisible(n.knob(\"JitterPattern\").value() == \"Custom\")\n    n\['endl'].setVisible(n.knob(\"JitterPattern\").value() == \"Custom\")\n"  
label "\[value JitterPattern]"  
xpos -861  
ypos -270  
mapsize {0.15 0.15}  
addUserKnob {20 jittermizer}  
addUserKnob {41 channels T IDistort.channels}  
addUserKnob {4 JitterPattern l "Jitter Pattern" M {"Kodak 5248" "Kodak 5279" "Kodak FX214" "Kodak GTS274" "Kodak 5217" "Kodak 5218" Custom "" ""}}  
JitterPattern Custom  
addUserKnob {6 BBOX l "use BBox" -STARTLINE}  
BBOX true  
addUserKnob {26 ""}  
addUserKnob {14 size R 0 100}  
size {{noise_breakup}}  
addUserKnob {14 irregularity R 0 100}  
addUserKnob {14 intensity R 0 100}  
intensity 0.1  
addUserKnob {26 endl l "" +STARTLINE}  
addUserKnob {7 seed t "use the expression \"frame\" to put some running noise through the jittermizer. Otherwise it will be static." R 1 100}  
seed 1  
addUserKnob {41 uv_scale l amplitude T IDistort.uv_scale}  
addUserKnob {7 offset}  
offset 0.5  
addUserKnob {7 dilate R -100 100}  
addUserKnob {26 ""}  
addUserKnob {6 Crop l "Crop to Format" t "Crops the output to the project settings width & height" +STARTLINE}  
addUserKnob {20 Transform}  
addUserKnob {12 translate}  
addUserKnob {7 rotate}  
addUserKnob {14 scale R 0 100}  
scale 1  
addUserKnob {7 skew}  
addUserKnob {12 center}  
addUserKnob {20 Version}  
addUserKnob {26 Help l Help: T "Jittermizer can be used to add a bit of noise to an edge by \nrandomly breaking up the matte. For example, a roto-ed hair\nmatte that looks too straight can be made to look more ragged\nand natural with Jittermizer."}  
addUserKnob {26 ""}  
addUserKnob {32 MoreHelp l "More Help" T "start [https://confluence.lucasfilm.com/x/98uh](https://confluence.lucasfilm.com/x/98uh)" +STARTLINE}  
addUserKnob {26 ""}  
addUserKnob {26 VRelease l "Version Release:" T 8.0}  
addUserKnob {26 RNotes l ReleaseNotes: T "Updated Seed control so it drives Noise parameters\nSet seed to frame by default to get animated noise"}  
addUserKnob {26 "" l Authors: T "Jason Billington (jbillington)"}  
addUserKnob {26 last l "Last Edit By:" T "Sean Bittinger (sbittinger)"}  
addUserKnob {26 Date l "Release Date:" T 11/11/11}  
}  
Input {  
inputs 0  
name Input  
xpos 63  
ypos -988  
}  
Dilate {  
size {{parent.dilate i}}  
name Dilate  
xpos 63  
ypos -406  
}  
set N1f8d8c90 [stack 0]  
AdjBBox {  
numpixels {{(parent.IDistort.uv_scale/5)+35 i}}  
name AdjBBox1  
xpos 208  
ypos -399  
}  
Constant {  
inputs 0  
color 0.5  
format {{{parent.input.format}}}  
name Constant1  
xpos 366  
ypos -1059  
}  
Dot {  
name Dot2  
xpos 400  
ypos -953  
}  
set N1f8f4e80 [stack 0]  
push $N1f8f4e80  
Dot {  
name Dot3  
xpos 626  
ypos -953  
}  
NoOp {  
name Noise_Parameters  
xpos 592  
ypos -888  
addUserKnob {20 User}  
addUserKnob {3 seed}  
seed {{parent.seed i}}  
addUserKnob {26 ""}  
addUserKnob {7 w_size}  
w_size {{"\[switch \[numvalue parent.JitterPattern] \{0 \{format 3.3\} 1 \{format 2.7\} 2 \{format 1.87\} 3 \{format 0.04\} 4 \{format 0.23\} 5 \{format 0.1\} default \{value parent.size.w\}\}]" i}}  
addUserKnob {7 h_size}  
h_size {{"\[switch \[numvalue parent.JitterPattern] \{0 \{format 2.9\} 1 \{format 2.6\} 2 \{format 2.6\} 3 \{format 0.1\} 4 \{format 1.2\} 5 \{format 1.6\} default \{value parent.size.h\}\}]" i}}  
addUserKnob {26 ""}  
addUserKnob {7 w_irr}  
w_irr {{"\[switch \[numvalue parent.JitterPattern] \{0 \{format 0.6\} 1 \{format 1\} 2 \{format 1\} 3 \{format 0.9\} 4 \{format 0.6\} 5 \{format 0.6\} default \{value parent.irregularity.w\}\}]" i}}  
addUserKnob {7 h_irr}  
h_irr {{"\[switch \[numvalue parent.JitterPattern] \{0 \{format 0.6\} 1 \{format 0.76\} 2 \{format 0.76\} 3 \{format 0.76\} 4 \{format 0.86\} 5 \{format 0.86\} default \{value parent.irregularity.h\}\}]" i}}  
addUserKnob {26 ""}  
addUserKnob {7 w_mag}  
w_mag {{"\[switch \[numvalue parent.JitterPattern] \{0 \{format 0.42\} 1 \{format 0.37\} 2 \{format 0.41\} 3 \{format 0.49\} 4 \{format 0.48\} 5 \{format 0.38\} default \{value parent.intensity.w\}\}]" i}}  
addUserKnob {7 h_mag}  
h_mag {{"\[switch \[numvalue parent.JitterPattern] \{0 \{format 0.46\} 1 \{format 0.6\} 2 \{format 0.6\} 3 \{format 0.5\} 4 \{format 0.42\} 5 \{format 0.17\} default \{value parent.intensity.h\}\}]" i}}  
}  
Noise {  
output {rgba.red -rgba.green -rgba.blue -rgba.alpha}  
cliptype none  
replace true  
size {{parent.Noise_Parameters.w_size i}}  
zoffset {{1+parent.Noise_Parameters.seed+parent.Noise_Parameters.w_irr/2 i}}  
octaves 2  
gamma 1  
translate {{parent.translate i} {parent.translate i}}  
rotate {{parent.rotate i}}  
scale {{parent.scale i}}  
skewX {{parent.skew i}}  
center {{parent.center i} {parent.center i}}  
xrotate {{parent.Noise_Parameters.w_irr*45 i}}  
yrotate {{parent.Noise_Parameters.w_irr*45 i}}  
name red  
xpos 592  
ypos -812  
}  
Noise {  
output {-rgba.red rgba.green -rgba.blue -rgba.alpha}  
cliptype none  
replace true  
size {{parent.Noise_Parameters.h_size}}  
zoffset {{1+2*parent.Noise_Parameters.seed+parent.Noise_Parameters.h_irr/2}}  
octaves 2  
gamma 1  
translate {{parent.translate} {parent.translate}}  
rotate {{parent.rotate}}  
scale {{parent.scale}}  
skewX {{parent.skew}}  
center {{parent.center} {parent.center}}  
xrotate {{parent.Noise_Parameters.h_irr*45}}  
yrotate {{parent.Noise_Parameters.h_irr*45}}  
name green  
xpos 592  
ypos -770  
}  
Multiply {  
channels {rgba.red rgba.green -rgba.blue none}  
value 2  
name Multiply2  
xpos 592  
ypos -728  
}  
Add {  
channels {rgba.red rgba.green -rgba.blue none}  
value -1  
name Add2  
xpos 592  
ypos -685  
}  
Multiply {  
channels {rgba.red rgba.green -rgba.blue none}  
value {{parent.Noise_Parameters.w_mag/2 i} {parent.Noise_Parameters.h_mag/2 i} 1 1}  
name Multiply3  
xpos 592  
ypos -643  
}  
Merge2 {  
inputs 2  
operation plus  
Achannels {rgba.red rgba.green rgba.blue -rgba.alpha}  
Bchannels {rgba.red rgba.green rgba.blue -rgba.alpha}  
output {rgba.red rgba.green rgba.blue -rgba.alpha}  
name Plus  
xpos 366  
ypos -636  
}  
Clamp {  
maximum_enable false  
name Clamp1  
xpos 366  
ypos -608  
}  
Add {  
channels {rgba.red rgba.green -rgba.blue none}  
value 0.5  
name Add3  
xpos 366  
ypos -566  
}  
Multiply {  
channels rgb  
value 0.5  
name Multiply4  
xpos 366  
ypos -524  
}  
CopyBBox {  
inputs 2  
name CopyBBox1  
xpos 366  
ypos -399  
disable {{!parent.BBOX i}}  
}  
push $N1f8d8c90  
Dot {  
name Dot1  
xpos 97  
ypos -294  
}  
Copy {  
inputs 2  
from0 rgba.red  
to0 backward.u  
from1 rgba.green  
to1 backward.v  
bbox A  
name Copy  
xpos 366  
ypos -313  
}  
IDistort {  
channels rgb  
uv backward  
uv_offset {{parent.offset}}  
uv_scale {{noise_amp}}  
blur_scale 85  
name IDistort  
xpos 366  
ypos -241  
}  
Remove {  
channels backward  
name Remove  
xpos 366  
ypos -179  
}  
Crop {  
box {{0 i} {0 i} {width i} {height i}}  
reformat true  
crop false  
name Crop  
xpos 366  
ypos -113  
disable {{!parent.Crop i}}  
}  
Output {  
name Output1  
xpos 366  
ypos 70  
}  
Noise {  
inputs 0  
output {-rgba.red rgba.green -rgba.blue -rgba.alpha}  
cliptype none  
replace true  
size {{parent.Noise_Parameters.h_size}}  
zoffset {{2*parent.Noise_Parameters.seed+parent.Noise_Parameters.h_irr/2}}  
octaves 2  
gamma 1  
translate {{parent.translate} {parent.translate}}  
rotate {{parent.rotate}}  
scale {{parent.scale}}  
skewX {{parent.skew}}  
center {{parent.center} {parent.center}}  
xrotate {{parent.Noise_Parameters.w_irr*45}}  
yrotate {{parent.Noise_Parameters.w_irr*45}}  
name green1  
xpos 742  
ypos -772  
}  
end_group  
set Nc4483b10 [stack 0]  
push 0  
Constant {  
inputs 0  
channels rgb  
format "255 255 0 0 255 255 1 square 255"  
name Constant2  
xpos -1418  
ypos -1348  
}  
Group {  
name Flare2  
xpos -1418  
ypos -1256  
addUserKnob {20 User}  
addUserKnob {26 text l "" +STARTLINE T "Hidden position for display purposes"}  
addUserKnob {41 position +INVISIBLE T Flare2.position}  
addUserKnob {41 size_mult l "size mult" T Flare2.size_mult}  
addUserKnob {41 anamorph T Flare2.anamorph}  
addUserKnob {41 corners_angle l angle T Flare2.corners_angle}  
addUserKnob {41 corners -STARTLINE T Flare2.corners}  
addUserKnob {41 flattening l "edge flattening" T Flare2.flattening}  
addUserKnob {7 corner_rotation l Rotation R -180 180}  
addUserKnob {7 edgeflatting l "Edge Flattening" R 0 2}  
}  
Input {  
inputs 0  
name Input1  
xpos -588  
ypos -243  
}  
Flare {  
position {{width/2} {height/2}}  
radius {80 100 125.53}  
size_mult {{(ILM_Kernel.Size)}}  
anamorph {{(-0.3*(ILM_Kernel.PAR))+1.3}}  
ring_color 0  
inner_falloff 0  
corners 8  
flattening {{NoOp2.constraint*edgeflatting}}  
sharpness 0.999  
corners_angle {{TeardropCTRL.angleDegrees+corner_rotation}}  
name Flare2  
xpos -588  
ypos -203  
}  
Output {  
name Output1  
xpos -588  
ypos -103  
}  
end_group  
Expression {  
expr0 r*2  
expr1 r*2  
expr2 r*2  
expr3 clamp(r*2)  
name Expression7  
xpos -1418  
ypos -1210  
}  
Transform {  
scale {{"1-parent.NoOp2.apBladeCenterW * (NoOp2.manualc ? (NoOp2.capblade_switch_w ? NoOp2.abconstrainw : 1) : NoOp2.filterc )"} {"1-parent.NoOp2.apBladeCenterH * (NoOp2.manualc ? (NoOp2.capblade_switch_h ? NoOp2.abconstrainh : 1) : NoOp2.filterc )"}}  
center {{width/2} {height/2}}  
shutteroffset centred  
name Transform7  
xpos -1418  
ypos -1156  
}  
Transform {  
translate {{"(lerp(width+width*NoOp2.ratioFromCentre, -width+width*NoOp2.ratioFromCentre, NoOp1.UVStepperNormalised.x)*NoOp2.apBladeW*NoOp2.sliceCurveControl.w) * (NoOp2.manualc ? (NoOp2.capblade_switch_x ? NoOp2.abconstrainx : 1) : NoOp2.filterc )"} {"(lerp(height+height*NoOp2.ratioFromCentre, -height+height*NoOp2.ratioFromCentre, NoOp1.UVStepperNormalised.y)*NoOp2.apBladeH*NoOp2.sliceCurveControl.h) * (NoOp2.manualc ? (NoOp2.capblade_switch_y ? NoOp2.abconstrainy : 1) : NoOp2.filterc )"}}  
center {960 540}  
shutteroffset centred  
name Transform12  
xpos -1418  
ypos -1110  
}  
Blur {  
channels rgba  
size {{parent.blurbladeocclu}}  
name Blur15  
xpos -1418  
ypos -1052  
}  
Dot {  
name Dot55  
xpos -1384  
ypos -1014  
}  
Constant {  
inputs 0  
channels rgb  
format "255 255 0 0 255 255 1 square 255"  
name Constant1  
xpos -461  
ypos -3900  
}  
Flare {  
position {{width/2} {height/2}}  
radius {80 100 125.53}  
size_mult {{(ILM_Kernel.Size)}}  
ring_color 0  
inner_falloff 0  
corners 11  
flattening {{4-(3*(ILM_Kernel.EdgeFlattening))}}  
sharpness 0.999  
corners_angle 38  
name Flare1  
xpos -461  
ypos -3770  
}  
set N4415e2a0 [stack 0]  
Dot {  
name Dot44  
xpos -427  
ypos -3690  
}  
set N441766b0 [stack 0]  
Dot {  
name Dot48  
xpos -588  
ypos -3690  
}  
Input {  
inputs 0  
name Library_Kernel  
note_font_size 21  
xpos -996  
ypos -3740  
}  
Reformat {  
format "255 255 0 0 255 255 1 square 255"  
name Reformat1  
xpos -996  
ypos -3626  
}  
CopyBBox {  
inputs 2  
name CopyBBox1  
xpos -622  
ypos -3620  
}  
push $N441766b0  
Switch {  
inputs 2  
which {{Switch}}  
name Switch3  
xpos -461  
ypos -3620  
}  
NoTimeBlur {  
name NoTimeBlur1  
xpos -461  
ypos -3479  
}  
Dot {  
name Dot57  
xpos -427  
ypos -3427  
}  
set N7a364e00 [stack 0]  
Dot {  
name Dot58  
xpos -589  
ypos -3427  
}  
set N7a369e80 [stack 0]  
Dot {  
name Dot60  
xpos -804  
ypos -3427  
}  
set N7a36edd0 [stack 0]  
Dot {  
name Dot2  
xpos -1097  
ypos -3427  
}  
Dot {  
name Dot23  
xpos -1097  
ypos -2208  
}  
push $N7a36edd0  
Ramp {  
replace true  
p0 {0 0}  
p1 {255 0}  
type smooth1  
name Ramp4  
xpos -838  
ypos -3409  
}  
Transform {  
scale {-1 1}  
center {127.5 127.5}  
black_outside false  
shutteroffset centred  
name ReverseTeardrop  
xpos -838  
ypos -3373  
disable {{TeardropCTRL.teardrop>0}}  
}  
Crop {  
box {0 0 255 255}  
crop false  
name Crop3  
xpos -838  
ypos -3337  
}  
Grade {  
channels alpha  
gamma {{TeardropCTRL.pointiness}}  
reverse true  
black_clamp false  
name Pointiness  
xpos -838  
ypos -3302  
}  
Dot {  
name Dot61  
xpos -804  
ypos -3262  
}  
push $N7a369e80  
Crop {  
box {-5000 -5000 5255 5255}  
name Crop25  
xpos -623  
ypos -3389  
}  
Expression {  
expr0 (x+0.5)/width  
expr1 (y+0.5)/height  
expr2 0  
name Expression6  
xpos -623  
ypos -3366  
}  
set N450a97a0 [stack 0]  
Transform {  
scale {1 {"TeardropCTRL.teardrop>0 ? 1-TeardropCTRL.teardropRegion : 1+TeardropCTRL.teardropRegion"}}  
center {127 127}  
name Transform29  
xpos -731  
ypos -3310  
}  
push $N450a97a0  
Dissolve {  
inputs 2+1  
channels rgb  
which 1  
name Dissolve16  
xpos -623  
ypos -3266  
}  
Crop {  
box {0 0 255 255}  
crop false  
name Crop26  
xpos -623  
ypos -3197  
}  
Dot {  
name Dot47  
xpos -589  
ypos -3134  
}  
push $N7a364e00  
STMap {  
inputs 2  
channels rgba  
uv rgb  
name STMap2  
xpos -461  
ypos -3138  
}  
Transform {  
scale {1 {1-TeardropCTRL.squashRegion*0.99}}  
center {127.5 127.5}  
shutteroffset centred  
name Squash  
xpos -461  
ypos -3090  
}  
Transform {  
scale {{1-TeardropCTRL.stretchRegion*0.99} 1}  
center {127.5 127.5}  
shutteroffset centred  
name Stretch  
xpos -461  
ypos -3046  
}  
Transform {  
rotate {{TeardropCTRL.angleDegrees}}  
center {127.5 127.5}  
shutteroffset centred  
name Transform2  
xpos -461  
ypos -3002  
}  
push $N7a364e00  
Dot {  
name Dot52  
xpos -217  
ypos -3427  
}  
Dot {  
name Dot53  
xpos -217  
ypos -2926  
}  
set N888f0240 [stack 0]  
Switch {  
inputs 2  
which {{tearswitch}}  
name Switch8  
label "tearing switch"  
xpos -461  
ypos -2930  
}  
push $N888f0240  
Switch {  
inputs 2  
which 1  
name Switch14  
label "swirlybokeh switch"  
xpos -461  
ypos -2860  
disable {{-ILM_Kernel.Switch2+1}}  
}  
Dot {  
name Dot51  
xpos -427  
ypos -2758  
}  
set N889057c0 [stack 0]  
Dot {  
name Dot50  
xpos -598  
ypos -2758  
}  
Dot {  
name Dot49  
xpos -598  
ypos -2255  
}  
push $N889057c0  
Transform {  
scale {{(-0.3*(ILM_Kernel.PAR))+1.3} {1/((-0.3*(ILM_Kernel.PAR))+1.3)}}  
center {127.5 127.5}  
shutteroffset centred  
name Aspect  
xpos -461  
ypos -2702  
}  
Dot {  
name Dot24  
xpos -427  
ypos -2627  
}  
set N88922ee0 [stack 0]  
Dot {  
name Dot20  
xpos 64  
ypos -2627  
}  
Expression {  
expr0 r*2  
expr1 r*2  
expr2 r*2  
expr3 clamp(r*2)  
name Expression5  
xpos 30  
ypos -2546  
}  
Dot {  
name Dot21  
xpos 64  
ypos -2507  
}  
set N8893ac40 [stack 0]  
Erode {  
size -6  
name Erode5  
xpos -41  
ypos -2444  
}  
Blur {  
channels rgba  
size 8  
name Blur5  
xpos -41  
ypos -2386  
}  
push $N8893ac40  
Erode {  
size 10  
name Erode6  
xpos 97  
ypos -2440  
}  
Blur {  
channels rgba  
size {{esmooth}}  
name Blur9  
xpos 92  
ypos -2382  
}  
ChannelMerge {  
inputs 2  
operation out  
name ChannelMerge2  
xpos 30  
ypos -2324  
}  
push $N88922ee0  
Expression {  
expr0 (x+0.5)/width  
expr1 (y+0.5)/height  
expr2 0  
name Expression1  
xpos -232  
ypos -2411  
}  
set Nf469b340 [stack 0]  
Transform {  
rotate {{skewing}}  
center {127.5 127.5}  
black_outside false  
name Transform1  
xpos -350  
ypos -2355  
}  
push $Nf469b340  
Keymix {  
inputs 3  
channels rgba  
name Keymix1  
xpos -232  
ypos -2311  
}  
push $N88922ee0  
STMap {  
inputs 2  
channels rgba  
uv rgb  
name STMap1  
xpos -461  
ypos -2311  
disable {{-skew_switch+1}}  
}  
Switch {  
inputs 2  
which {{Switch}}  
name Switch7  
label "Input Kernel"  
xpos -461  
ypos -2265  
}  
CopyBBox {  
inputs 2  
name CopyBBox2  
xpos -461  
ypos -2212  
}  
Dot {  
name Dot43  
xpos -427  
ypos -1448  
}  
set Nf46dbff0 [stack 0]  
Dot {  
name Dot34  
xpos -936  
ypos -1448  
}  
set Nf46e0f70 [stack 0]  
Dot {  
name Dot35  
xpos -1189  
ypos -1448  
}  
Expression {  
expr0 r*2  
expr1 r*2  
expr2 r*2  
expr3 clamp(r*2)  
name Expression4  
xpos -1223  
ypos -1286  
}  
Transform {  
scale {{"1-parent.NoOp2.filterSliceCentreW * (NoOp2.manualc ? (NoOp2.cselfocclusion_switch_w ? NoOp2.soconstrainw : 1) : NoOp2.filterc )"} {"1-parent.NoOp2.filterSliceCentreH * (NoOp2.manualc ? (NoOp2.cselfocclusion_switch_h ? NoOp2.soconstrainh : 1) : NoOp2.filterc )"}}  
center {{width/2} {height/2}}  
shutteroffset centred  
name Transform9  
xpos -1223  
ypos -1229  
}  
Transform {  
translate {{"(lerp(width+width*NoOp2.ratioFromCentre, -width+width*NoOp2.ratioFromCentre, NoOp1.UVStepperNormalised.x)*NoOp2.filterSliceW*NoOp2.sliceCurveControl.w) * (NoOp2.manualc ? (NoOp2.cselfocclusion_switch_x ? NoOp2.soconstrainx : 1) : NoOp2.filterc )"} {"(lerp(height+height*NoOp2.ratioFromCentre, -height+height*NoOp2.ratioFromCentre, NoOp1.UVStepperNormalised.y)*NoOp2.filterSliceH*NoOp2.sliceCurveControl.h) * (NoOp2.manualc ? (NoOp2.cselfocclusion_switch_y ? NoOp2.soconstrainy : 1) : NoOp2.filterc )"}}  
center {960 540}  
shutteroffset centred  
name Transform4  
xpos -1223  
ypos -1183  
}  
Blur {  
channels rgba  
size {{parent.blurocclu}}  
name Blur14  
xpos -1223  
ypos -1121  
}  
Dot {  
name Dot36  
xpos -1189  
ypos -1075  
}  
push $Nf46e0f70  
Remove {  
operation keep  
channels none  
name Remove3  
xpos -970  
ypos -1420  
}  
Dot {  
name Dot37  
xpos -936  
ypos -1372  
}  
set Nb05cb890 [stack 0]  
Dot {  
name Dot38  
xpos -1057  
ypos -1372  
}  
Crop {  
box {0 0 {width} {height}}  
crop false  
name Crop1  
xpos -1091  
ypos -1347  
}  
Expression {  
expr3 1  
name Expression8  
xpos -1091  
ypos -1321  
}  
Crop {  
box {0 0 {width} {height}}  
softness {{NoOp2.barndoorSoftness*box.r}}  
crop false  
name Crop5  
xpos -1091  
ypos -1275  
}  
Transform {  
scale {{"1-parent.NoOp2.barnDoorCentreW * (NoOp2.manualc ? (NoOp2.cbarndoor_switch_w ? NoOp2.bconstrainw : 1) : NoOp2.filterc )"} {"1-parent.NoOp2.barnDoorCentreH * (NoOp2.manualc ? (NoOp2.cbarndoor_switch_h ? NoOp2.bconstrainh : 1) : NoOp2.filterc )"}}  
center {{width/2} {height/2}}  
shutteroffset centred  
name Transform5  
xpos -1091  
ypos -1244  
}  
Transform {  
translate {{"(lerp(width+width*NoOp2.ratioFromCentre, -width+width*NoOp2.ratioFromCentre, NoOp1.UVStepperNormalised.x)*NoOp2.barnDoorW*NoOp2.sliceCurveControl.w) * (NoOp2.manualc ? (NoOp2.cbarndoor_switch_x ? NoOp2.bconstrainx : 1) : NoOp2.filterc )"} {"(lerp(height+height*NoOp2.ratioFromCentre, -height+height*NoOp2.ratioFromCentre, NoOp1.UVStepperNormalised.y)*NoOp2.barnDoorH*NoOp2.sliceCurveControl.w) * (NoOp2.manualc ? (NoOp2.cbarndoor_switch_y ? NoOp2.bconstrainy : 1) : NoOp2.filterc )"}}  
center {960 540}  
shutteroffset centred  
name Transform6  
xpos -1091  
ypos -1198  
}  
Dot {  
name Dot39  
xpos -1057  
ypos -1150  
}  
push $Nb05cb890  
Radial {  
output alpha  
replace true  
area {0 0 {width} {height}}  
softness {{parent.NoOp2.catseyeSoftness}}  
name Radial1  
xpos -970  
ypos -1341  
}  
Transform {  
scale {{"1-parent.NoOp2.catseyeCentreW  * (NoOp2.manualc ? (NoOp2.ccatseye_switch_w ? NoOp2.cconstrainw : 1) : NoOp2.filterc )"} {"1-parent.NoOp2.catseyeCentreH * (NoOp2.manualc ? (NoOp2.ccatseye_switch_h ? NoOp2.cconstrainh : 1) : NoOp2.filterc )"}}  
center {{width/2} {height/2}}  
shutteroffset centred  
name Transform8  
xpos -970  
ypos -1283  
}  
Transform {  
translate {{"(lerp(width+width*NoOp2.ratioFromCentre, -width+width*NoOp2.ratioFromCentre, NoOp1.UVStepperNormalised.x)*NoOp2.catseyeW*NoOp2.sliceCurveControl.w) * (NoOp2.manualc ? (NoOp2.ccatseye_switch_x ? NoOp2.cconstrainx : 1) : NoOp2.filterc )"} {"(lerp(height+height*NoOp2.ratioFromCentre, -height+height*NoOp2.ratioFromCentre, NoOp1.UVStepperNormalised.y)*NoOp2.catseyeH*NoOp2.sliceCurveControl.h) * (NoOp2.manualc ? (NoOp2.ccatseye_switch_y ? NoOp2.cconstrainy : 1) : NoOp2.filterc )"}}  
center {960 540}  
shutteroffset centred  
name Transform10  
xpos -970  
ypos -1237  
}  
Dot {  
name Dot40  
xpos -936  
ypos -1200  
}  
push $Nf46dbff0  
Dot {  
name Dot28  
xpos -427  
ypos -1282  
}  
set N440f1b40 [stack 0]  
Dot {  
name Dot41  
xpos -590  
ypos -1282  
}  
set N440f6930 [stack 0]  
Merge2 {  
inputs 2  
operation mask  
bbox intersection  
also_merge all  
name Merge12  
label "bbox = \[value bbox]\ncatseye"  
xpos -751  
ypos -1216  
}  
push $N440f6930  
Switch {  
inputs 2  
which {{catseyeswitch}}  
name Switch9  
xpos -624  
ypos -1204  
}  
set N4410c150 [stack 0]  
Merge2 {  
inputs 2  
operation mask  
bbox intersection  
also_merge all  
name Merge13  
label "bbox = \[value bbox]\nbarndoor"  
xpos -743  
ypos -1160  
}  
push $N4410c150  
Switch {  
inputs 2  
which {{barndoorswitch}}  
name Switch10  
xpos -624  
ypos -1148  
}  
set N3c3b0a00 [stack 0]  
Merge2 {  
inputs 2  
operation mask  
bbox intersection  
name Merge15  
label "bbox = \[value bbox]\nself-occlusion"  
xpos -746  
ypos -1091  
}  
push $N3c3b0a00  
Switch {  
inputs 2  
which {{selfoccluswitch}}  
name Switch11  
xpos -624  
ypos -1079  
}  
set N3c3c9580 [stack 0]  
Merge2 {  
inputs 2  
operation mask  
bbox intersection  
name Merge1  
label "bbox = \[value bbox]\naperture blade"  
xpos -743  
ypos -1028  
}  
push $N3c3c9580  
Switch {  
inputs 2  
which {{apbladewitch}}  
name Switch12  
xpos -624  
ypos -1016  
}  
Dot {  
name Dot42  
xpos -590  
ypos -930  
}  
push $N440f1b40  
Switch {  
inputs 2  
which 1  
name Switch4  
label "swirlybokeh switch"  
xpos -461  
ypos -940  
disable {{-ILM_Kernel.Switch2+1}}  
}  
Dot {  
name Dot4  
xpos -427  
ypos -720  
}  
set N3c3f8c90 [stack 0]  
Dot {  
name Dot5  
xpos -1360  
ypos -720  
}  
Dot {  
name Dot3  
xpos -1363  
ypos 2292  
}  
push $N3c3f8c90  
Expression {  
expr3 r*2  
name Expression2  
xpos 556  
ypos -724  
}  
Dot {  
name Dot10  
xpos 590  
ypos -201  
}  
set N3c415c60 [stack 0]  
Dot {  
name Dot8  
xpos 590  
ypos 204  
}  
set N3c41ace0 [stack 0]  
Dot {  
name Dot59  
xpos 591  
ypos 478  
}  
set N445a2870 [stack 0]  
Dot {  
name Dot11  
xpos 591  
ypos 864  
}  
set N445a77c0 [stack 0]  
Dot {  
name Dot6  
xpos 591  
ypos 962  
}  
set N445ac710 [stack 0]  
push $N445a77c0  
ChannelMerge {  
inputs 2  
operation stencil  
name ChannelMerge4  
xpos 273  
ypos 946  
}  
Median {  
channels alpha  
size 1  
ignore_top_line false  
name Median1  
xpos 273  
ypos 1040  
}  
Dot {  
name Dot45  
xpos 307  
ypos 1197  
}  
set N445ca410 [stack 0]  
Blur {  
channels alpha  
size {{"ILM_Kernel.Radial1.softness == 0  ? 0 :(ILM_Kernel.Radial1.softness > 0) ? 5 : 0"}}  
name Blur2  
xpos 167  
ypos 1260  
}  
push $N445ca410  
Switch {  
inputs 2  
which {{catseyeswitch}}  
name Switch1  
xpos 273  
ypos 1266  
}  
set N445dd210 [stack 0]  
Blur {  
channels alpha  
size {{parent.NoOp2.barndoorSoftness*20}}  
name Blur10  
xpos 167  
ypos 1343  
}  
push $N445dd210  
Switch {  
inputs 2  
which {{barndoorswitch}}  
name Switch5  
xpos 273  
ypos 1349  
}  
set N445f31c0 [stack 0]  
Blur {  
channels alpha  
size {{parent.blurocclu}}  
name Blur16  
xpos 170  
ypos 1423  
}  
push $N445f31c0  
Switch {  
inputs 2  
which {{selfoccluswitch}}  
name Switch6  
xpos 273  
ypos 1429  
}  
set N44609210 [stack 0]  
Blur {  
channels alpha  
size {{parent.blurbladeocclu}}  
name Blur17  
xpos 163  
ypos 1490  
}  
push $N44609210  
Switch {  
inputs 2  
which {{apbladewitch}}  
name Switch13  
xpos 273  
ypos 1496  
}  
Expression {  
expr3 ceil(a)  
name Expression9  
xpos 273  
ypos 1552  
}  
Blur {  
channels alpha  
size 20  
name Blur4  
xpos 273  
ypos 1598  
}  
push $N445a77c0  
Dot {  
name Dot26  
xpos 901  
ypos 864  
}  
Transform {  
translate {{50*sin(radians(ILM_Kernel.angle_1))} {50*cos(radians(ILM_Kernel.angle_1))}}  
center {127.5 127.5}  
name Transform  
label angle  
xpos 864  
ypos 1450  
}  
Erode {  
size {{-erode_size}}  
name Erode7  
xpos 864  
ypos 1600  
}  
Blur {  
channels rgba  
size {{falloff}}  
name Blur7  
xpos 864  
ypos 1659  
}  
push $N445ac710  
Erode {  
size {{(ILM_Kernel.Erode3.size)*-1}}  
name Erode2  
xpos 617  
ypos 1128  
}  
Blur {  
channels rgba  
size {{ILM_Kernel.Ringblur}}  
name Blur1  
xpos 617  
ypos 1247  
}  
push $N445ac710  
Erode {  
size {{ILM_Kernel.Ringsize}}  
name Erode3  
xpos 500  
ypos 1172  
}  
set N74e0ae60 [stack 0]  
Blur {  
channels rgba  
size {{ILM_Kernel.InsideRingBlur}}  
name Blur6  
xpos 500  
ypos 1243  
}  
ChannelMerge {  
inputs 2  
operation out  
name ChannelMerge1  
xpos 554  
ypos 1307  
}  
Merge2 {  
inputs 2  
operation mask  
bbox B  
name Merge2  
xpos 554  
ypos 1659  
disable {{-enable_angle+1}}  
}  
Multiply {  
inputs 1+1  
value 0  
name Multiply2  
xpos 273  
ypos 1656  
}  
Dot {  
name Dot56  
xpos 135  
ypos 1663  
}  
set N1d3ef9b0 [stack 0]  
push $N1d3ef9b0  
Erode {  
size -0.6  
name Erode4  
xpos -70  
ypos 1451  
}  
Blur {  
channels rgba  
size 3.4  
name Blur19  
xpos -173  
ypos 1457  
}  
Transform {  
scale 0.97  
center {127.5 127.5}  
shutteroffset centred  
name Transform11  
xpos -282  
ypos 1457  
}  
push $N74e0ae60  
Dot {  
name Dot27  
xpos -34  
ypos 1182  
}  
set N1d41d310 [stack 0]  
Blur {  
channels rgba  
size 6  
name Blur12  
xpos -68  
ypos 1223  
}  
push $N1d41d310  
push $N445a77c0  
Dot {  
name Dot7  
xpos -173  
ypos 864  
}  
set N74b5ee30 [stack 0]  
Dot {  
name Dot14  
xpos -173  
ypos 918  
}  
Reformat {  
inputs 0  
format "255 255 0 0 255 255 1 square 255"  
name Reformat2  
xpos -881  
ypos 459  
}  
Noise {  
size {{spechlg_scale}}  
zoffset {{spec_seed}}  
gamma 0.765  
translate {{-width/2+(NoOp1.animatedCropRegion.r+NoOp1.animatedCropRegion.x)/2} {-height/2+(NoOp1.animatedCropRegion.t+NoOp1.animatedCropRegion.y)/2}}  
scale 2  
center {{width/2} {width/2}}  
name Noise3  
xpos -881  
ypos 518  
}  
Grade {  
channels rgba  
blackpoint 0.71  
white_clamp true  
name Grade4  
xpos -880  
ypos 580  
}  
Dot {  
name Dot30  
xpos -846  
ypos 640  
}  
set N74b9dec0 [stack 0]  
Blur {  
channels rgba  
size 8  
name Blur13  
xpos -1040  
ypos 636  
}  
set N74ba2f40 [stack 0]  
Group {  
name Splice  
xpos -1152  
ypos 673  
addUserKnob {20 Splice}  
addUserKnob {7 Center}  
Center 0.05  
addUserKnob {7 Front}  
Front 0.04  
addUserKnob {7 FrontGamma}  
FrontGamma 1  
addUserKnob {7 Back}  
Back 0.03  
addUserKnob {7 BackGamma}  
BackGamma 1  
}  
Input {  
inputs 0  
name Input1  
xpos -430  
ypos -594  
}  
set N74bbb030 [stack 0]  
Grade {  
channels rgba  
blackpoint {{whitepoint-parent.Back}}  
whitepoint {{parent.Center}}  
gamma {{parent.BackGamma}}  
white_clamp true  
name BackSlice  
xpos -293  
ypos -538  
}  
push $N74bbb030  
Grade {  
channels rgba  
blackpoint {{parent.Front+whitepoint}}  
whitepoint {{parent.Center}}  
gamma {{parent.FrontGamma}}  
white_clamp true  
name FrontSplice  
xpos -430  
ypos -538  
}  
Merge2 {  
inputs 2  
operation min  
name Min  
xpos -430  
ypos -497  
}  
Output {  
name Output1  
xpos -430  
ypos -454  
}  
end_group  
push $N74ba2f40  
Group {  
name Splice1  
xpos -1040  
ypos 676  
addUserKnob {20 Splice}  
addUserKnob {7 Center}  
Center 0.15  
addUserKnob {7 Front}  
Front 0.07  
addUserKnob {7 FrontGamma}  
FrontGamma 1  
addUserKnob {7 Back}  
Back 0.02  
addUserKnob {7 BackGamma}  
BackGamma 1  
}  
Input {  
inputs 0  
name Input1  
xpos -430  
ypos -594  
}  
set N7428be10 [stack 0]  
Grade {  
channels rgba  
blackpoint {{whitepoint-parent.Back}}  
whitepoint {{parent.Center}}  
gamma {{parent.BackGamma}}  
white_clamp true  
name BackSlice  
xpos -333  
ypos -538  
}  
push $N7428be10  
Grade {  
channels rgba  
blackpoint {{parent.Front+whitepoint}}  
whitepoint {{parent.Center}}  
gamma {{parent.FrontGamma}}  
white_clamp true  
name FrontSplice  
xpos -430  
ypos -542  
}  
Merge2 {  
inputs 2  
operation min  
name Min  
xpos -430  
ypos -497  
}  
Output {  
name Output1  
xpos -430  
ypos -454  
}  
end_group  
Merge2 {  
inputs 2  
name Merge3  
xpos -1040  
ypos 712  
}  
push $N74b9dec0  
Merge2 {  
inputs 2  
mix 0.35  
name Merge7  
xpos -880  
ypos 712  
}  
Shuffle {  
red alpha  
green alpha  
blue alpha  
name Shuffle1  
label "\[value in]-->\[value out]"  
xpos -880  
ypos 854  
}  
set N3c5f11e0 [stack 0]  
Dot {  
name Dot15  
xpos -846  
ypos 918  
}  
push $N445a2870  
Transform {  
scale 0.7  
center {{(-width/2+(NoOp1.animatedCropRegion.r+NoOp1.animatedCropRegion.x)/2)+152.6} {(-height/2+(NoOp1.animatedCropRegion.t+NoOp1.animatedCropRegion.y)/2)+147}}  
shutteroffset centred  
name Transform13  
xpos 47  
ypos 474  
}  
Erode {  
size 8.5  
name Erode1  
xpos -89  
ypos 476  
}  
Blur {  
channels rgba  
size 60  
name Blur18  
xpos -234  
ypos 482  
}  
push $N3c41ace0  
Dot {  
name Dot16  
xpos 87  
ypos 204  
}  
set N3c62fa20 [stack 0]  
Dot {  
name Dot17  
xpos 87  
ypos 258  
}  
Reformat {  
inputs 0  
format "255 255 0 0 255 255 1 square 255"  
name Reformat4  
xpos -859  
ypos 77  
}  
Noise {  
size {{dust_size}}  
zoffset {{dust_seed}}  
octaves 6  
lacunarity 1.64  
gain 1  
gamma 0.742  
center {1078 1134}  
name Noise1  
xpos -859  
ypos 132  
}  
Sharpen {  
channels rgba  
size 7  
name Sharpen3  
xpos -859  
ypos 200  
}  
set N9c700570 [stack 0]  
push $N3c415c60  
push $Nc4483b10  
Dot {  
name Dot9  
xpos -827  
ypos -201  
}  
push $N3c3f8c90  
Grade {  
white {{gain}}  
name Grade1  
xpos -461  
ypos -605  
}  
Merge2 {  
inputs 2+1  
bbox B  
mix {{ripples}}  
name Merge6  
xpos -461  
ypos -205  
disable {{-ripple_switch+1}}  
}  
Dot {  
name Dot18  
xpos -427  
ypos 38  
}  
set N9c736000 [stack 0]  
Dot {  
name Dot19  
xpos -528  
ypos 38  
}  
set N9c73b010 [stack 0]  
Merge2 {  
inputs 2+1  
bbox B  
name Merge9  
xpos -562  
ypos 254  
}  
push 0  
push $N9c73b010  
push $N3c62fa20  
push $N9c700570  
push $N9c736000  
Merge2 {  
inputs 2+1  
operation stencil  
bbox B  
name Merge10  
xpos -461  
ypos 200  
}  
Dissolve {  
inputs 3+1  
which {{dust+1}}  
name Dissolve2  
xpos -461  
ypos 339  
}  
Grade {  
inputs 1+1  
name Grade2  
xpos -461  
ypos 482  
}  
Dot {  
name Dot12  
xpos -426  
ypos 698  
}  
set N21d72e80 [stack 0]  
Dot {  
name Dot13  
xpos -527  
ypos 698  
}  
set N21d77f00 [stack 0]  
Merge2 {  
inputs 2+1  
bbox B  
name Merge8  
xpos -561  
ypos 914  
}  
push 0  
push $N21d77f00  
push $N74b5ee30  
push $N3c5f11e0  
push $N21d72e80  
Merge2 {  
inputs 2+1  
operation stencil  
bbox B  
name Merge4  
xpos -460  
ypos 860  
}  
Dissolve {  
inputs 3+1  
which {{spechlg+1}}  
name Dissolve1  
xpos -460  
ypos 996  
}  
Blur {  
inputs 1+1  
channels rgb  
size {{ILM_Kernel.EdgeBlur}}  
invert_mask true  
name Blur3  
xpos -460  
ypos 1177  
}  
Blur {  
inputs 1+1  
channels rgb  
size {{ILM_Kernel.EdgeBlur/2}}  
invert_mask true  
name Blur11  
xpos -460  
ypos 1223  
}  
Grade {  
inputs 1+1  
white 0  
mix 0.31  
name Grade3  
xpos -460  
ypos 1457  
}  
Dot {  
name Dot46  
xpos -425  
ypos 1619  
}  
set N21dd5750 [stack 0]  
Dot {  
name Dot29  
xpos -278  
ypos 1619  
}  
set N21dda7d0 [stack 0]  
Expression {  
expr0 1  
expr1 1  
expr2 1  
expr3 1  
name Expression_all_1  
xpos -74  
ypos 1615  
}  
Copy {  
inputs 2  
from0 rgba.alpha  
to0 rgba.alpha  
name Copy1  
xpos -74  
ypos 1653  
}  
Premult {  
name Premult1  
xpos -74  
ypos 1714  
}  
set Nc635f40 [stack 0]  
Multiply {  
channels rgba  
value {{ILM_Kernel.OnionRing}}  
name Multiply1  
xpos -74  
ypos 1826  
}  
push $N21dda7d0  
Merge2 {  
inputs 2  
name Merge5  
label "UNION (was B)"  
xpos -312  
ypos 1820  
}  
push 0  
push $N21dda7d0  
push $Nc635f40  
Transform {  
scale {0.97 0.98}  
center {127.5 127.5}  
shutteroffset centred  
name Transform3  
xpos -229  
ypos 1714  
}  
push $N21dd5750  
Merge2 {  
inputs 2  
operation stencil  
bbox B  
mix 0.8  
name Merge11  
xpos -462  
ypos 1714  
}  
Dissolve {  
inputs 3+1  
which {{ILM_Kernel.OnionRing+1}}  
name Dissolve3  
xpos -462  
ypos 1820  
}  
Dot {  
name Dot33  
xpos -428  
ypos 1907  
}  
set Nc684d80 [stack 0]  
Dot {  
name Dot32  
xpos -281  
ypos 2059  
}  
push $Nc684d80  
Group {  
name GodRays1  
label "adaptive center"  
xpos -462  
ypos 2002  
addUserKnob {20 User}  
addUserKnob {41 channels T GodRays1.channels}  
addUserKnob {41 translate T GodRays1.translate}  
addUserKnob {41 scale T GodRays1.scale}  
addUserKnob {41 center T GodRays1.center}  
addUserKnob {7 xbias l "X bias" R 0.4 1.6}  
xbias 0.2  
addUserKnob {7 ybias l "Y Bias" R 0.4 1.6}  
ybias 1  
}  
Input {  
inputs 0  
name Input1  
xpos -414  
ypos 311  
}  
GodRays {  
channels {-rgba.red rgba.green rgba.blue rgba.alpha}  
translate {{"cascale == 1 ? 0 : ca*10"} 0}  
scale {{lerp(1,1.1,(cascale*ca))}}  
center {{"(input.bbox.x + input.bbox.w/2)*xbias"} {"(input.bbox.y + input.bbox.h/2)*ybias"}}  
gamma 3.35  
name GodRays1  
xpos -414  
ypos 357  
disable {{-enableca+1}}  
}  
Output {  
name Output1  
xpos -414  
ypos 451  
}  
end_group  
CopyBBox {  
inputs 2  
name CopyBBox3  
xpos -462  
ypos 2102  
}  
Switch {  
inputs 2  
which {{Switch}}  
name Switch2  
xpos -459  
ypos 2288  
}  
Remove {  
operation keep  
channels rgb  
name Remove1  
xpos -459  
ypos 2334  
}  
Keyer {  
operation "luminance key"  
name Keyer1  
xpos -459  
ypos 2380  
}  
add_layer {sliceMatte sliceMatte.a}  
Expression {  
channel0 sliceMatte  
expr0 a  
channel1 none  
channel2 none  
name Expression3  
label "sliceMatte from alpha"  
xpos -459  
ypos 2427  
}  
Group {  
inputs 3  
name Transform_SizeCompensation  
tile_color 0xc088ddff  
xpos -460  
ypos 2498  
addUserKnob {20 User}  
addUserKnob {7 scalecompensation l Scale t "This hack helps balancing the scale of the kernel for extreme cases (wide angle lenses or vintage lenses with strong field curvature)" R 1 3}  
scalecompensation 2  
}  
Input {  
inputs 0  
name Input1  
xpos -413  
ypos 143  
}  
Transform {  
scale {{"max(NoOp2.constraint*scalecompensation, 1)"}}  
center {127.5 127.5}  
shutteroffset centred  
name Transform_SizeCompensation  
xpos -413  
ypos 189  
disable {{-ILM_Kernel.Switch2+1}}  
addUserKnob {20 User}  
}  
Output {  
name Output1  
xpos -413  
ypos 283  
}  
end_group  
Transform {  
translate {{-width/2+(NoOp1.animatedCropRegion.r+NoOp1.animatedCropRegion.x)/2} {-height/2+(NoOp1.animatedCropRegion.t+NoOp1.animatedCropRegion.y)/2}}  
scale {{min(NoOp1.boundsDivided.h/height,NoOp1.boundsDivided.w/width)}}  
center {{width/2} {height/2}}  
shutteroffset centred  
name Transform_ChrisMagic  
xpos -460  
ypos 2559  
disable true  
}  
Transform {  
translate {{-width/2+(NoOp1.animatedCropRegion.r+NoOp1.animatedCropRegion.x)/2} {-height/2+(NoOp1.animatedCropRegion.t+NoOp1.animatedCropRegion.y)/2}}  
scale {{min(NoOp1.boundsDivided.h/height,NoOp1.boundsDivided.w/width)}}  
center {{width/2} {height/2}}  
invert_matrix true  
shutteroffset centred  
name Transform_ChrisMagic_invert  
xpos -460  
ypos 2611  
disable true  
}  
Dot {  
name DIMATTE  
label "do not delete"  
note_font_size 25  
xpos -429  
ypos 2686  
}  
Output {  
name Output1  
xpos -463  
ypos 3620  
}  
StickyNote {  
inputs 0  
name StickyNote3  
tile_color 0xdb5a08ff  
label "edge blur"  
note_font_size 12  
note_font_color 0xffffffff  
xpos -566  
ypos 1177  
}  
StickyNote {  
inputs 0  
name StickyNote4  
tile_color 0xdb5a08ff  
label "onion ring"  
note_font_size 12  
note_font_color 0xffffffff  
xpos -582  
ypos 1791  
}  
StickyNote {  
inputs 0  
name StickyNote8  
tile_color 0xdb5a08ff  
label "Barn door"  
note_font_size 12  
note_font_color 0xffffffff  
xpos -529  
ypos -1154  
}  
StickyNote {  
inputs 0  
name StickyNote6  
tile_color 0xdb5a08ff  
label "Chris's Cats eye gag"  
note_font_size 12  
note_font_color 0xffffffff  
xpos -619  
ypos -1330  
}  
StickyNote {  
inputs 0  
name StickyNote1  
tile_color 0xdb5a08ff  
label Ripple  
note_font_size 12  
note_font_color 0xffffffff  
xpos -373  
ypos -274  
}  
StickyNote {  
inputs 0  
name StickyNote9  
tile_color 0xdb5a08ff  
label Catseye  
note_font_size 12  
note_font_color 0xffffffff  
xpos -525  
ypos -1198  
}  
StickyNote {  
inputs 0  
name StickyNote14  
tile_color 0xdb5a08ff  
label "Bbox is set to intersection,\ndo not change it or Chromab will break"  
note_font_size 12  
note_font_color 0xffffffff  
xpos -348  
ypos -1148  
}  
StickyNote {  
inputs 0  
name StickyNote12  
tile_color 0xdb5a08ff  
label chromab  
note_font_size 12  
note_font_color 0xffffffff  
xpos -598  
ypos 2014  
}  
StickyNote {  
inputs 0  
name StickyNote7  
tile_color 0xdb5a08ff  
label "Cooke lenses"  
note_font_size 12  
note_font_color 0xffffffff  
xpos -522  
ypos -2400  
}  
NoOp {  
inputs 0  
name Ripple_Settings  
xpos -1061  
ypos -362  
addUserKnob {20 User}  
addUserKnob {7 size R 1 100}  
size {{ripple_amount}}  
addUserKnob {12 center}  
center {127.5 127.5}  
}  
StickyNote {  
inputs 0  
name StickyNote13  
tile_color 0xdb5a08ff  
label "Kill softened edge"  
note_font_size 12  
note_font_color 0xffffffff  
xpos 158  
ypos 1609  
}  
StickyNote {  
inputs 0  
name StickyNote10  
tile_color 0xdb5a08ff  
label Self-Occlusion  
note_font_size 12  
note_font_color 0xffffffff  
xpos -535  
ypos -1097  
}  
StickyNote {  
inputs 0  
name StickyNote15  
tile_color 0xdb5a08ff  
label ApertureBlade  
note_font_size 12  
note_font_color 0xffffffff  
xpos -529  
ypos -1022  
}  
Input {  
inputs 0  
name Image  
xpos -1906  
ypos -1752  
number 1  
}  
Dot {  
name Dot31  
selected true  
xpos -1872  
ypos -1634  
}  
set N883cb3e0 [stack 0]  
Dot {  
name Dot1  
xpos -1739  
ypos -1632  
}  
Crop {  
box {{"parent.NoOp1.animatedCropRegion - (NoOp1.UVStepperNormalised.x == 0 ? 0 : softness.w/2)"} {"parent.NoOp1.animatedCropRegion - (NoOp1.UVStepperNormalised.y == 0 ? 0 : softness.h/2)"} {"parent.NoOp1.animatedCropRegion + (NoOp1.UVStepperNormalised.x == 1 ? 0 : softness.w/2)"} {"parent.NoOp1.animatedCropRegion + (NoOp1.UVStepperNormalised.y == 1 ? 0 : softness.h/2)"}}  
softness {{softness_1}}  
name Crop2  
xpos -1773  
ypos -1542  
addUserKnob {20 User}  
addUserKnob {7 softness_1 l Softness R 40 200}  
softness_1 120  
}  
push $N883cb3e0  
ColorLookup {  
lut {master {}  
  red {curve C 1 x0.5 1 1}  
  green {curve C 1 x0.5 1 1}  
  blue {}  
  alpha {}}  
name ColorLookup1  
xpos -1906  
ypos -1550  
}  
push $N883cb3e0  
Dot {  
name Dot22  
xpos -1982  
ypos -1634  
}  
set N883f0ed0 [stack 0]  
NoOp {  
name NoOp1  
label "crop controls"  
xpos -2016  
ypos -1556  
addUserKnob {20 User}  
addUserKnob {14 UVStepper R 0 100}  
UVStepper {{(floor((frame*cellDivisions.v)%1*cellDivisions.u))/cellDivisions.u} {floor((frame%1)*cellDivisions.v)/cellDivisions.v}}  
addUserKnob {12 UVStepperNormalised}  
UVStepperNormalised {{"cellDivisions.u > 1 ? UVStepper.w*(cellDivisions.u)/(cellDivisions.u-1) : 1"} {"cellDivisions.v > 1 ? UVStepper.h*(cellDivisions.v)/(cellDivisions.v-1) : 1"}}  
addUserKnob {15 animatedCropRegion}  
animatedCropRegion {{bbox.x+boundsDivided.w*UVStepper.w*cellDivisions.u} {bbox.y+boundsDivided.h*UVStepper.h*cellDivisions.v} {animatedCropRegion.x+boundsDivided.w} {animatedCropRegion.y+boundsDivided.h}}  
addUserKnob {14 boundsDivided R 0 100}  
boundsDivided {{(bbox.r-bbox.x)/cellDivisions.u} {(bbox.t-bbox.y)/cellDivisions.v}}  
addUserKnob {20 tab}  
addUserKnob {30 cellDivisions}  
cellDivisions {{ILM_Kernel.cellDivisions.u} {ILM_Kernel.cellDivisions.v}}  
}  
push $N883f0ed0  
Dot {  
name Dot25  
xpos -2087  
ypos -1634  
}  
set N88180c70 [stack 0]  
Dot {  
name Dot54  
xpos -2200  
ypos -1634  
}  
NoOp {  
name TeardropCTRL  
label "and more"  
xpos -2234  
ypos -1556  
addUserKnob {20 User}  
addUserKnob {26 text l "" +STARTLINE T "Hidden calculation for general display purposes"}  
addUserKnob {12 p1 +INVISIBLE}  
p1 {{parent.NoOp2.occlusionCentre} {parent.NoOp2.occlusionCentre}}  
addUserKnob {12 p2 +INVISIBLE}  
p2 {{(parent.NoOp1.animatedCropRegion.x+parent.NoOp1.animatedCropRegion.r)/2} {(parent.NoOp1.animatedCropRegion.y+parent.NoOp1.animatedCropRegion.t)/2}}  
addUserKnob {7 Xdiff +INVISIBLE}  
Xdiff {{p1.x-p2.x}}  
addUserKnob {7 Ydiff +INVISIBLE}  
Ydiff {{p1.y-p2.y}}  
addUserKnob {7 distance l "distance from center" +INVISIBLE}  
distance {{"sqrt(pow(p2.x - p1.x, 2) + pow(p2.y - p1.y, 2))"}}  
addUserKnob {7 maxDistance l "max distance" +INVISIBLE}  
maxDistance {{"sqrt(pow(p1.x, 2) + pow(p1.y, 2))"}}  
addUserKnob {7 amount +INVISIBLE}  
amount {{distance/maxDistance}}  
addUserKnob {26 ""}  
addUserKnob {7 angle +INVISIBLE}  
angle {{atan2(Ydiff,Xdiff)}}  
addUserKnob {7 angleDegrees +INVISIBLE}  
angleDegrees {{degrees(angle)*angle_mult}}  
addUserKnob {7 angle_mult l "angle multiplier" +INVISIBLE R 0 2}  
angle_mult 1  
addUserKnob {26 ""}  
addUserKnob {7 teardrop R -1 1}  
addUserKnob {7 pointiness R 0.2 5}  
pointiness 1  
addUserKnob {7 squash}  
addUserKnob {7 stretch}  
addUserKnob {26 ""}  
addUserKnob {7 teardropRegion l "teardrop amount for region" +INVISIBLE}  
teardropRegion {{teardrop*amount}}  
addUserKnob {7 squashRegion l "squash amount for region" +INVISIBLE}  
squashRegion {{squash*amount}}  
addUserKnob {7 stretchRegion l "stretch amount for region" +INVISIBLE}  
stretchRegion {{stretch*amount}}  
}  
StickyNote {  
inputs 0  
name StickyNote5  
tile_color 0xdb5a08ff  
label "Specular Highlights"  
note_font_size 12  
note_font_color 0xffffffff  
xpos -371  
ypos 745  
}  
StickyNote {  
inputs 0  
name StickyNote11  
tile_color 0xdb5a08ff  
label "flash center"  
note_font_size 12  
note_font_color 0xffffffff  
xpos -588  
ypos 474  
}  
StickyNote {  
inputs 0  
name StickyNote16  
tile_color 0xdb5a08ff  
label "Onion shadow"  
note_font_size 12  
note_font_color 0xffffffff  
xpos -597  
ypos 1464  
}  
push $N88180c70  
NoOp {  
name NoOp2  
label "slice controls\nand constraints"  
xpos -2121  
ypos -1562  
addUserKnob {20 User}  
addUserKnob {6 mirrorTheCurves l mirrorCurve t "Link from mirrorcurve in the Advanced tab" +STARTLINE}  
addUserKnob {14 sliceCurveControl -STARTLINE R 0 100}  
sliceCurveControl {{"mirrorTheCurves ? ColorLookup1.lut.red(NoOp1.UVStepperNormalised.x < 0.5 ? NoOp1.UVStepperNormalised.x : 1-NoOp1.UVStepperNormalised.x) : ColorLookup1.lut.red(NoOp1.UVStepperNormalised.x)"} {"mirrorTheCurves ?  ColorLookup1.lut.green(NoOp1.UVStepperNormalised.y < 0.5 ? NoOp1.UVStepperNormalised.y : 1-NoOp1.UVStepperNormalised.y) : ColorLookup1.lut.green(NoOp1.UVStepperNormalised.y)"}}  
addUserKnob {26 ""}  
addUserKnob {14 ratioFromCentre l "ratio from centre" +HIDDEN R 0 100}  
ratioFromCentre {{(occlusionCentre-width/2)/(width/2)} {(occlusionCentre-height/2)/(height/2)}}  
addUserKnob {12 occlusionCentre l "occlusion centre" +INVISIBLE}  
occlusionCentre {{width/2} {height/2}}  
addUserKnob {26 ""}  
addUserKnob {20 group1 l "Occlusion modes" n 1}  
group1 0  
addUserKnob {7 catseyeW R -1 1}  
catseyeW -0.2  
addUserKnob {7 catseyeH -STARTLINE R -1 1}  
catseyeH -0.2  
addUserKnob {7 catseyeCentreW}  
catseyeCentreW -0.1  
addUserKnob {7 catseyeCentreH}  
catseyeCentreH -0.4  
addUserKnob {7 catseyeSoftness}  
addUserKnob {26 ""}  
addUserKnob {7 barnDoorW R -1 1}  
addUserKnob {7 barnDoorH -STARTLINE R -1 1}  
addUserKnob {7 barnDoorCentreW}  
addUserKnob {7 barnDoorCentreH}  
addUserKnob {7 barndoorSoftness}  
addUserKnob {26 ""}  
addUserKnob {7 filterSliceW R -1 1}  
addUserKnob {7 filterSliceH -STARTLINE R -1 1}  
addUserKnob {7 filterSliceCentreW}  
addUserKnob {7 filterSliceCentreH}  
addUserKnob {26 ""}  
addUserKnob {7 apBladeW R -1 1}  
addUserKnob {7 apBladeH -STARTLINE R -1 1}  
addUserKnob {7 apBladeCenterW}  
addUserKnob {7 apBladeCenterH}  
addUserKnob {26 ""}  
addUserKnob {7 radialDistortion1 l "radial distortion 1" R -0.3 0.3}  
addUserKnob {7 radialDistortion2 l "radial distortion 2" R -0.1 0.1}  
addUserKnob {20 endGroup_1 l endGroup n -1}  
addUserKnob {20 group2 l Constraint n 1}  
group2 0  
addUserKnob {7 constraint l Constraint t "max distance calculation to constraint center occlusion to be close to 0 (none)"}  
constraint {{"clamp(pow((ILM_Kernel.TeardropCTRL.distance/ILM_Kernel.TeardropCTRL.maxDistance), cmult))"}}  
addUserKnob {7 cmult +INVISIBLE R 1 2}  
cmult 1  
addUserKnob {26 ""}  
addUserKnob {26 text l "" +STARTLINE T Catseye}  
addUserKnob {6 ccatseye_switch_x l "X Offset" +STARTLINE}  
ccatseye_switch_x true  
addUserKnob {6 ccatseye_switch_y l "Y Offset" -STARTLINE}  
ccatseye_switch_y true  
addUserKnob {6 ccatseye_switch_w l "Scale W" -STARTLINE}  
ccatseye_switch_w true  
addUserKnob {6 ccatseye_switch_h l "Scale H" -STARTLINE}  
ccatseye_switch_h true  
addUserKnob {7 ccatseyex l "X Offset"}  
ccatseyex 2  
addUserKnob {7 cconstrainx -STARTLINE}  
cconstrainx {{"clamp(pow(constraint, ccatseyex))"}}  
addUserKnob {7 ccatseyey l "Y Offset"}  
addUserKnob {7 cconstrainy -STARTLINE}  
cconstrainy {{"clamp(pow(constraint, ccatseyey))"}}  
addUserKnob {7 ccatseyew l "Scale W" R 0 5}  
ccatseyew 5  
addUserKnob {7 cconstrainw -STARTLINE}  
cconstrainw {{"clamp(pow(constraint, ccatseyew))"}}  
addUserKnob {7 ccatseyeh l "Scale H" R 0 5}  
ccatseyeh 1  
addUserKnob {7 cconstrainh -STARTLINE}  
cconstrainh {{"clamp(pow(constraint, ccatseyeh))"}}  
addUserKnob {26 ""}  
addUserKnob {26 text_1 l "" +STARTLINE T Barndoor}  
addUserKnob {6 cbarndoor_switch_x l "X Offset" +STARTLINE}  
cbarndoor_switch_x true  
addUserKnob {6 cbarndoor_switch_y l "Y Offset" -STARTLINE}  
cbarndoor_switch_y true  
addUserKnob {6 cbarndoor_switch_w l "Scale W" -STARTLINE}  
cbarndoor_switch_w true  
addUserKnob {6 cbarndoor_switch_h l "Scale H" -STARTLINE}  
cbarndoor_switch_h true  
addUserKnob {7 cbarndoorx l "X Offset"}  
addUserKnob {7 bconstrainx -STARTLINE}  
bconstrainx {{"clamp(pow(constraint, cbarndoorx))"}}  
addUserKnob {7 cbarndoory l "Y Offset"}  
addUserKnob {7 bconstrainy -STARTLINE}  
bconstrainy {{"clamp(pow(constraint, cbarndoory))"}}  
addUserKnob {7 cbarndoorw l "Scale W" R 0 5}  
addUserKnob {7 bconstrainw -STARTLINE}  
bconstrainw {{"clamp(pow(constraint, cbarndoorw))"}}  
addUserKnob {7 cbarndoorh l "Scale H" R 0 5}  
addUserKnob {7 bconstrainh -STARTLINE R 0 2}  
bconstrainh {{"clamp(pow(constraint, cbarndoorh))"}}  
addUserKnob {26 ""}  
addUserKnob {26 text_2 l "" +STARTLINE T SelfOcclusion}  
addUserKnob {6 cselfocclusion_switch_x l "X Offset" +STARTLINE}  
cselfocclusion_switch_x true  
addUserKnob {6 cselfocclusion_switch_y l "Y Offset" -STARTLINE}  
cselfocclusion_switch_y true  
addUserKnob {6 cselfocclusion_switch_w l "Scale W" -STARTLINE}  
cselfocclusion_switch_w true  
addUserKnob {6 cselfocclusion_switch_h l "Scale H" -STARTLINE}  
cselfocclusion_switch_h true  
addUserKnob {7 cselfocclux l "X Offset"}  
addUserKnob {7 soconstrainx -STARTLINE}  
soconstrainx {{"clamp(pow(constraint, cselfocclux))"}}  
addUserKnob {7 cselfoccluy l "Y Offset"}  
addUserKnob {7 soconstrainy -STARTLINE}  
soconstrainy {{"clamp(pow(constraint, cselfoccluy))"}}  
addUserKnob {7 cselfoccluw l "Scale W" R 0 5}  
addUserKnob {7 soconstrainw -STARTLINE}  
soconstrainw {{"clamp(pow(constraint, cselfoccluw))"}}  
addUserKnob {7 cselfoccluh l "Scale H" R 0 5}  
addUserKnob {7 soconstrainh -STARTLINE}  
soconstrainh {{"clamp(pow(constraint, cselfoccluh))"}}  
addUserKnob {26 ""}  
addUserKnob {26 text_3 l "" +STARTLINE T "Aperture Blade"}  
addUserKnob {6 capblade_switch_x l "X Offset" +STARTLINE}  
capblade_switch_x true  
addUserKnob {6 capblade_switch_y l "Y Offset" -STARTLINE}  
capblade_switch_y true  
addUserKnob {6 capblade_switch_w l "Scale W" -STARTLINE}  
capblade_switch_w true  
addUserKnob {6 capblade_switch_h l "Scale H" -STARTLINE}  
capblade_switch_h true  
addUserKnob {7 capbladex l "X Offset"}  
addUserKnob {7 abconstrainx -STARTLINE}  
abconstrainx {{"clamp(pow(constraint, capbladex))"}}  
addUserKnob {7 capbladey l "Y Offset"}  
addUserKnob {7 abconstrainy -STARTLINE}  
abconstrainy {{"clamp(pow(constraint, capbladey))"}}  
addUserKnob {7 capbladew l "Scale W" R 0 5}  
addUserKnob {7 abconstrainw -STARTLINE}  
abconstrainw {{"clamp(pow(constraint, capbladew))"}}  
addUserKnob {7 capbladeh l "Scale H" R 0 5}  
addUserKnob {7 abconstrainh -STARTLINE}  
abconstrainh {{"clamp(pow(constraint, capbladeh))"}}  
addUserKnob {26 ""}  
addUserKnob {26 text_4 l "" +STARTLINE T "Tearing (not active)"}  
addUserKnob {6 tearing_switch l Tearing +STARTLINE}  
addUserKnob {6 squash_switch l Squash -STARTLINE}  
addUserKnob {6 stretch_switch l Stretch -STARTLINE}  
addUserKnob {7 ctearing l Tearing R 0 5}  
addUserKnob {7 tconstrain -STARTLINE}  
tconstrain {{"pow(constraint, ctearing)"}}  
addUserKnob {7 csquash l Squash R 0 5}  
addUserKnob {7 sqconstrain -STARTLINE}  
sqconstrain {{"pow(constraint, csquash)"}}  
addUserKnob {7 cstretch l Stretch R 0 5}  
addUserKnob {7 stconstrain -STARTLINE}  
stconstrain {{"pow(constraint, cstretch)"}}  
addUserKnob {20 endGroup n -1}  
addUserKnob {6 manualc l "Manual constraint (overwrite smoothness filter)" +STARTLINE}  
manualc true  
addUserKnob {20 filters n 1}  
filters 0  
addUserKnob {4 filter l "Filter type" M {none linear plinear smooth0 smooth1 smooth "" "" ""}}  
addUserKnob {7 filterc l filter -STARTLINE}  
filterc {{"(filter == 0 ? 1 : filter == 1 ? linear : filter == 2 ? plinear : filter == 3 ? smooth0: filter == 4 ? smooth1 : filter == 5 ? smooth : 1)"}}  
addUserKnob {7 linear}  
linear {{"pow(constraint, 1)"}}  
addUserKnob {7 plinear}  
plinear {{"pow(constraint, 2.75)"}}  
addUserKnob {7 smooth0}  
smooth0 {{"lerp(0,1, pow(constraint,1.5))"}}  
addUserKnob {7 smooth1}  
smooth1 {{"smoothstep(0, 1, constraint)"}}  
addUserKnob {7 smooth}  
smooth {{"pow(constraint, 3)*(constraint*(constraint*6-15)+10)"}}  
addUserKnob {20 endGroup_2 l endGroup n -1}  
}  
push $N4415e2a0  
Viewer {  
frame_range 1001-1013  
masking_ratio 1.85  
viewerProcess "Shot (ACES)"  
name Viewer1  
xpos -351  
ypos -3427  
addUserKnob {20 Lock}  
addUserKnob {6 lock_all_buffers l "lock all buffers" -STARTLINE}  
addUserKnob {6 lock_buffer_1 l "lock buffer 1" +STARTLINE}  
addUserKnob {6 lock_buffer_2 l "lock buffer 2" +STARTLINE}  
addUserKnob {6 lock_buffer_3 l "lock buffer 3" +STARTLINE}  
addUserKnob {6 lock_buffer_4 l "lock buffer 4" +STARTLINE}  
addUserKnob {6 lock_buffer_5 l "lock buffer 5" +STARTLINE}  
addUserKnob {6 lock_buffer_6 l "lock buffer 6" +STARTLINE}  
addUserKnob {6 lock_buffer_7 l "lock buffer 7" +STARTLINE}  
addUserKnob {6 lock_buffer_8 l "lock buffer 8" +STARTLINE}  
addUserKnob {6 lock_buffer_9 l "lock buffer 9" +STARTLINE}  
addUserKnob {6 lock_buffer_0 l "lock buffer 0" +STARTLINE}  
}  
end_group  
Dot {  
name Dot80  
selected true  
xpos -1617  
ypos 11776  
}  
Camera3 {  
inputs 0  
focal 55  
haperture 47.29443972  
vaperture 25.58  
focal_point 10  
fstop 2.8  
name Camera11  
selected true  
xpos -886  
ypos 11752  
}  
push $N309cc860  
Group {  
name ILM_Swirly_Crop  
tile_color 0x242e57ff  
note_font_size 21  
note_font_color 0xffb300ff  
selected true  
xpos -1389  
ypos 11643  
addUserKnob {20 User}  
addUserKnob {26 text_1 l "<b><font color=#ff0000>DO NOT BREAK THE LINK</font><b>"}  
addUserKnob {26 text l "<b>What am I doing?" T "When the SwirlyBokeh is enable, this crop allows the defocus \nto convolve each splits individualy rather than defocusing \nthe whole image multiple times a frame.\n\nAdditionaly, this node uses a NoTimeBlur which prevents \nany animated node from getting processed\nby the TimeBlur_Divisions below.\n"}  
addUserKnob {26 ""}  
addUserKnob {26 text_2 l " " T "Kernel (SwirlyBokeh) is driving this group."}  
addUserKnob {26 ""}  
addUserKnob {41 box +INVISIBLE T Crop.box}  
addUserKnob {6 enable +INVISIBLE +STARTLINE}  
enable {{parent.ILM_Kernel.Switch2}}  
addUserKnob {26 type -STARTLINE T sb.crop}  
}  
Input {  
inputs 0  
name Input1  
xpos -588  
ypos 153  
}  
set Nc44c18d0 [stack 0]  
NoTimeBlur {  
name NoTimeBlur  
xpos -725  
ypos 202  
}  
Expression {  
channel0 sliceMatte  
expr0 1  
channel1 none  
channel2 none  
channel3 {none none none -rgba.alpha}  
name Expression  
label "sliceMatte from alpha"  
xpos -725  
ypos 248  
}  
Crop {  
box {{parent.parent.ILM_Kernel.Crop2.box-ILM_Kernel.Crop2.softness.w} {parent.parent.ILM_Kernel.Crop2.box-ILM_Kernel.Crop2.softness.h} {parent.parent.ILM_Kernel.Crop2.box+ILM_Kernel.Crop2.softness.w} {parent.parent.ILM_Kernel.Crop2.box+ILM_Kernel.Crop2.softness.h}}  
name Crop  
note_font_color 0xffb300ff  
xpos -725  
ypos 318  
}  
push $Nc44c18d0  
Switch {  
inputs 2  
which {{enable}}  
name Switch1  
xpos -588  
ypos 318  
}  
Output {  
name Output1  
xpos -588  
ypos 379  
}  
end_group  
Group {  
inputs 3  
name ZDefocusILM  
help "\n A wrapper around ZDefocus that\n attempts to calculate the defocus\n value for all points in a depth\n pass with a given lens geometry.\n \n Assumes a valid depth value for all pixels.\n Pixels without a depth value (Z=0) will be set\n to 'max Z'\n \n \n math:\n \n N = F-stop\n f = focal length\n flm = focal length multiplier\n hap = filmback width\n u = focal distance\n d = depth\n \n m = f / fabs(u - f)\n coc = (f * m / N) * fabs(d - u) / d\n defocus = coc/hap*flm*width/2\n "  
knobChanged "\nn = nuke.thisNode()\nk = nuke.thisKnob()\n\nif [k.name](http://k.name)() == 'output':\n        n.knobs()\['show_image'].setEnabled(k.value() == 'focal plane setup')\n        \nif [k.name](http://k.name)() == 'useCam':\n    if k.value() == True:\n        n.knobs()\['focalLength'].setExpression('\[topnode input1].focal')\n        n.knobs()\['focalLength'].setEnabled(False)\n        n.knobs()\['hap'].setExpression('\[topnode input1].haperture')\n        n.knobs()\['hap'].setEnabled(False)\n    else:\n        n.knobs()\['focalLength'].clearAnimated()\n        n.knobs()\['focalLength'].setEnabled(True)\n        n.knobs()\['hap'].clearAnimated()\n        n.knobs()\['hap'].setEnabled(True)\n        \nif [k.name](http://k.name)() == 'autoLayerSpacing':\n        n.knobs()\['layers'].setEnabled(not k.value())\n        n.knobs()\['layerCurve'].setEnabled(not k.value())\n        \nif [k.name](http://k.name)() == 'filter_type':\n    if k.value() == 'disc':\n        n.knobs()\['shape'].setVisible(True)\n        n.knobs()\['aspect'].setVisible(True)\n        n.knobs()\['filter_channel'].setVisible(False)\n        n.knobs()\['use_input_channels'].setVisible(False)\n        n.knobs()\['filter_bounds'].setVisible(False)\n        n.knobs()\['blades'].setVisible(False)\n        n.knobs()\['roundness'].setVisible(False)\n        n.knobs()\['rotation'].setVisible(False)\n        n.knobs()\['inner_size'].setVisible(False)\n        n.knobs()\['inner_feather'].setVisible(False)\n        n.knobs()\['inner_brightness'].setVisible(False)\n        n.knobs()\['catadioptric_size'].setVisible(False)\n        n.knobs()\['catadioptric'].setVisible(False)\n    elif k.value() == 'bladed':\n        n.knobs()\['aspect'].setVisible(True)\n        n.knobs()\['shape'].setVisible(False)\n        n.knobs()\['filter_channel'].setVisible(False)\n        n.knobs()\['use_input_channels'].setVisible(False)\n        n.knobs()\['filter_bounds'].setVisible(False)\n        n.knobs()\['blades'].setVisible(True)\n        n.knobs()\['roundness'].setVisible(True)\n        n.knobs()\['rotation'].setVisible(True)\n        n.knobs()\['inner_size'].setVisible(True)\n        n.knobs()\['inner_feather'].setVisible(True)\n        n.knobs()\['inner_brightness'].setVisible(True)\n        n.knobs()\['catadioptric_size'].setVisible(True)\n        n.knobs()\['catadioptric'].setVisible(True)\n    elif k.value() == 'image':\n        n.knobs()\['aspect'].setVisible(False)\n        n.knobs()\['shape'].setVisible(False)\n        n.knobs()\['filter_channel'].setVisible(True)\n        n.knobs()\['use_input_channels'].setVisible(True)\n        n.knobs()\['filter_bounds'].setVisible(True)\n        n.knobs()\['blades'].setVisible(False)\n        n.knobs()\['roundness'].setVisible(False)\n        n.knobs()\['rotation'].setVisible(False)\n        n.knobs()\['inner_size'].setVisible(False)\n        n.knobs()\['inner_feather'].setVisible(False)\n        n.knobs()\['inner_brightness'].setVisible(False)\n        n.knobs()\['catadioptric_size'].setVisible(False)\n        n.knobs()\['catadioptric'].setVisible(False)\n        \nif [k.name](http://k.name)() == 'bloom':\n        n.knobs()\['bloom_threshold'].setEnabled(k.value())\n        n.knobs()\['bloom_gain'].setEnabled(k.value())\n\nif [k.name](http://k.name)() == 'cleanOutput':\n        n.knobs()\['minClamp'].setEnabled(k.value())\n"  
tile_color 0xff7f3fff  
label "\[value channels]"  
selected true  
xpos -1389  
ypos 11760  
addUserKnob {20 User l ZDefocusLens}  
addUserKnob {41 channels t "The effect is applied only to\nthese channels." T ZDefocus1.channels}  
addUserKnob {41 useGPUIfAvailable l "Use GPU if available" T ZDefocus1.useGPUIfAvailable}  
addUserKnob {26 ""}  
addUserKnob {4 resolution l Resolution t "Use lower resolutions for faster rendering." M {1:1 1:2 1:4 1:8 "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""}}  
addUserKnob {41 z_channel l "depth channel" T ZDefocus1.z_channel}  
addUserKnob {4 output t "<br><b>result</b> - show the result of the zblur<br><br><b>focal-plane setup</b> - output depth-of-field (DOF) info<br>to the rgb channels, where:<br>\t<b>red :</b> nearer to the camera than the focus plane,<br>\t<b>green:</b> in-focus area, within +/-<b>dof</b>/2 of the focus plane, and<br>\t<b>blue :</b> further from the camera than the focus plane.<br>\t<b>crosshatch :</b> areas that will have their blur values clamped by 'max blur'.<br><br><b>layer setup</b> - as focal plane setup, but showing the DOF info after the depth has been divided into layers. Pixels assigned to the same layer will have the same amount of blur applied to them.<br><br><b>filter shape setup</b> - display the filter shape in rgb.<br><br><b>Will always revert to 'result' in non-interactive mode (on the farm).</b>" M {result "focal plane setup" "layer setup" "filter shape setup" "" "" ""}}  
addUserKnob {41 show_image l "show image" -STARTLINE +DISABLED T ZDefocus1.show_image}  
addUserKnob {6 umultZ l "Unpremult Depth" t "Unpremult the depth channel.\n\nShould usually be on for 'depth.Z'\nand off for 'z'\n\nOnly for calculation - has no effect\non depth output." +STARTLINE}  
umultZ true  
addUserKnob {6 depthextend l "Depth Edge Extend" t "Uses Nuke EdgeExtend node to fill the depth map bsaed on the input alpha channel." -STARTLINE}  
addUserKnob {7 dof l "depth of field" t "Make a range of this depth entirely in focus. True theoretical depth-of-field would set this to 0." R 0 100}  
addUserKnob {41 blur_dof l "blur inside" -STARTLINE +INVISIBLE T ZDefocus1.blur_dof}  
addUserKnob {7 maxBlur l "max blur (disabled)" t "Maximum allowed blur. Has no effect on blur values at distances within the near/far max blur zone.\n\nThis value along with 'scaling' determines the defocus kernal size. To optimize, use the lowest value possible without clipping your subject out of the near/far max blur distances." +INVISIBLE R 0 100}  
maxBlur 40  
addUserKnob {7 maxsize l "max size" t "Maximum size of defocus, in pixels.\nUse this to prevent huge defocus values when objects are really close to camera." R 0 500}  
maxsize 100  
addUserKnob {7 scale l scaling t "Overall multiplier on to blur values." R 0.1 10}  
scale 1  
addUserKnob {26 ""}  
addUserKnob {6 useCam l "use camera input" t "Use camera input for focal length and horiz aperture." +STARTLINE}  
useCam true  
addUserKnob {7 fStop l F-stop t "F-stop value" R 0 32}  
fStop 2.8  
addUserKnob {7 focalLength l "focal length (mm)" t "Lens focal length.\n\n<b>Ignored if using the cam input.</b>" +DISABLED R 0 500}  
focalLength {{"\[topnode input1].focal"}}  
addUserKnob {7 flm l mult t "Focal length multiplier\n\nUseful when using non 35mm \"cropped\" filmbacks with 35mm lenses.\n\nThe horizontal aperture is multiplied by this value" -STARTLINE R 0 2}  
flm 1  
addUserKnob {7 hap l "horiz aperture (mm)" t "Horizontal aperture.\n\n<b>Ignored if using the cam input.</b>\n\nNOTE: if using values directly from the \"Film Ap Width\" field in Zeno, don't forget to convert from inches.\n\nmm. = in. * 25.4" +DISABLED R 0 100}  
hap {{"\[topnode input1].haperture"}}  
addUserKnob {41 center_1 l "focus plane (C)" T ZDefocus1.center}  
addUserKnob {6 sdecimal l "Focus pull decimal skip" t "Errors can be black boxes or stretched pixels \nwhen used with swirlybokeh.\nThis function prevent errors from happening  whenever the focus distance is animated.\n\n" -STARTLINE +INVISIBLE}  
sdecimal true  
addUserKnob {41 focal_point_1 l "focal point" T ZDefocus1.focal_point}  
addUserKnob {26 ""}  
addUserKnob {6 autoLayerSpacing l "automatic layer spacing" t "In automatic mode the plug-in will decide the number of depth layers to use and where to put them.\nTurn this off to control the depth layers manually." -STARTLINE}  
autoLayerSpacing true  
addUserKnob {41 layers l "depth layers" +DISABLED T ZDefocus1.layers}  
addUserKnob {41 layerCurve l "layer curve" +DISABLED T ZDefocus1.layerCurve}  
addUserKnob {26 ""}  
addUserKnob {4 filter_type l "filter type" M {disc bladed image}}  
filter_type image  
addUserKnob {41 shape l "filter shape" +HIDDEN T ZDefocus1.shape}  
addUserKnob {41 aspect l "aspect ratio" +HIDDEN T ZDefocus1.aspect}  
addUserKnob {41 blades +HIDDEN T ZDefocus1.blades}  
addUserKnob {41 roundness +HIDDEN T ZDefocus1.roundness}  
addUserKnob {41 rotation +HIDDEN T ZDefocus1.rotation}  
addUserKnob {41 inner_size l "inner size" +HIDDEN T ZDefocus1.inner_size}  
addUserKnob {41 inner_feather l "inner feather" +HIDDEN T ZDefocus1.inner_feather}  
addUserKnob {41 inner_brightness l "inner brightness" +HIDDEN T ZDefocus1.inner_brightness}  
addUserKnob {41 catadioptric_size l "catadioptric size" +HIDDEN T ZDefocus1.catadioptric_size}  
addUserKnob {41 catadioptric -STARTLINE +HIDDEN T ZDefocus1.catadioptric}  
addUserKnob {41 filter_channel l "filter channel" T ZDefocus1.filter_channel}  
addUserKnob {41 use_input_channels l "use input channels" -STARTLINE T ZDefocus1.use_input_channels}  
addUserKnob {41 filter_bounds l "filter bounds" T ZDefocus1.filter_bounds}  
addUserKnob {26 ""}  
addUserKnob {41 bloom_gamma l "gamma correction" T ZDefocus1.bloom_gamma}  
addUserKnob {6 bloom t "Enable blooming gain. Boost the highlights above the bloom threshold to make lens shape effects more visible." -STARTLINE}  
addUserKnob {41 bloom_threshold l "bloom threshold" +DISABLED T ZDefocus1.bloom_threshold}  
addUserKnob {41 bloom_gain l "bloom gain" +DISABLED T ZDefocus1.bloom_gain}  
addUserKnob {6 minClamp_panelDropped l "panel dropped state" +HIDDEN +STARTLINE}  
addUserKnob {26 ""}  
addUserKnob {20 Version}  
addUserKnob {26 _1 l Help: T "A wrapper around ZDefocus that attempts to calculate\nthe defocus value for all points in a depth pass\nwith a given lens geometry.\n\nUses an int expression to skip decimal values which is \nnecessary when paired with SwirlyBokeh."}  
addUserKnob {26 ""}  
addUserKnob {26 text l " " T "Based of Jason Porter ZDefoculens & Xavier Bourque PXF_zdefocus.\n\nEdited by jfernandez & fsanchez\n\t"}  
addUserKnob {26 ReleaseDate l "Release Date:" T 11/14/25}  
addUserKnob {26 ""}  
}  
Input {  
inputs 0  
name filter  
xpos -909  
ypos -167  
number 2  
}  
Reformat {  
type "to box"  
box_width 255  
box_height 255  
box_fixed true  
black_outside true  
pbb true  
name Reformat6  
label 255box  
xpos -909  
ypos 160  
}  
Input {  
inputs 0  
name img  
xpos -520  
ypos -538  
}  
Dot {  
name Dot1  
xpos -486  
ypos -402  
}  
set Na48919a0 [stack 0]  
Unpremult {  
channels depth  
name Unpremult  
xpos -520  
ypos -299  
disable {{1-parent.umultZ}}  
}  
EdgeExtend {  
channels depth  
detailAmount 0  
premultiply false  
name EdgeExtend1  
xpos -520  
ypos -205  
disable {{-depthextend+1}}  
}  
Reformat {  
type scale  
scale {{"resolution == 0 ? 1 : resolution == 1 ? 0.5 : resolution == 2 ? 0.25 : resolution == 3 ? 0.125 : 1"}}  
filter impulse  
pbb true  
name Reformat3  
xpos -520  
ypos -54  
disable {{"scale < 1 ? 0 : 1"}}  
}  
ZDefocus2 {  
inputs 2  
useGPUIfAvailable false  
math depth  
output {{"\$gui ? parent.output : 0"}}  
center 6  
dof {{parent.dof/parent.maxBlur x1001 0.02500000037}}  
blur_dof false  
size {{"NoOp1.defoAtInfinity * Reformat3.scale"}}  
max_size {{"parent.maxsize * Reformat3.scale"}}  
autoLayerSpacing {{parent.autoLayerSpacing}}  
filter_type {{parent.filter_type}}  
legacy_resize_mode false  
bloom {{parent.bloom}}  
name ZDefocus1  
xpos -520  
ypos 160  
}  
Reformat {  
type scale  
scale {{1/Reformat3.scale}}  
pbb true  
name Reformat2  
xpos -520  
ypos 328  
disable {{"scale > 1 ? 0 : 1"}}  
}  
Output {  
name Output1  
xpos -520  
ypos 504  
}  
Input {  
inputs 0  
name cam  
xpos -1090  
ypos -172  
number 1  
}  
push $Na48919a0  
NoOp {  
name NoOp1  
label depth_math  
xpos -744  
ypos -412  
addUserKnob {20 User}  
addUserKnob {7 scaling}  
scaling {{parent.scale}}  
addUserKnob {7 unitMult}  
unitMult 0.03280839895  
addUserKnob {26 ""}  
addUserKnob {26 text l "                " T "focus distance in cm"}  
addUserKnob {7 focalDistanceCM}  
focalDistanceCM {{"parent.center_1 / unitMult"}}  
addUserKnob {26 text_1 l "                " T "focus distance with int expression"}  
addUserKnob {7 focalDistanceINT}  
focalDistanceINT {{focalDistanceCM(int(frame))}}  
addUserKnob {26 text_2 l "                " T "focus distance result"}  
addUserKnob {7 focalDistance}  
focalDistance {{"sdecimal == 1 ? focalDistanceINT : focalDistanceCM"}}  
addUserKnob {26 ""}  
addUserKnob {7 reallyFar}  
reallyFar 10000000  
addUserKnob {7 defoAtInfinity}  
defoAtInfinity {{"((fabs(((focalLength*flm)/fStop)*(((reallyFar-(focalDistance*10))/reallyFar)*((focalLength*flm)/((focalDistance*10)-(focalLength*flm)))))) * (width/hap) / 2)*this.scaling"}}  
}  
end_group  
Group {  
name TimeBlur_Divisions11  
tile_color 0x242e57ff  
label "\[value divisions]"  
note_font_size 21  
note_font_color 0xffb300ff  
selected true  
xpos -1389  
ypos 11906  
addUserKnob {20 Infos l Info}  
addUserKnob {26 text l "<b><font color=#ff0000>DO NOT BREAK THE LINK</font><b>" -STARTLINE}  
addUserKnob {26 text_1 l "<b>What am I doing?</b>" T "The Timeblur node inside this group combines \nall the variable shapes into one image through \nthe sub-frame animated crop."}  
addUserKnob {26 ""}  
addUserKnob {26 Infos_2 l " " T "Kernel (SwirlyBokeh) is driving this group."}  
addUserKnob {26 ""}  
addUserKnob {20 User}  
addUserKnob {30 cellDivisions l Splits}  
cellDivisions {{parent.ILM_Kernel.cellDivisions} {parent.ILM_Kernel.cellDivisions}}  
addUserKnob {3 divisions l Divisions}  
divisions {{TimeBlur_Divisions11.cellDivisions.u*TimeBlur_Divisions11.cellDivisions.v}}  
addUserKnob {83 method l Method t "Warning!!! Linked to Kernel." +DISABLED M {TimeBlur TimeWarps}}  
method {{ILM_Kernel.method}}  
addUserKnob {26 ""}  
addUserKnob {41 box +INVISIBLE T Crop1.box}  
addUserKnob {41 softness +INVISIBLE T Crop1.softness}  
addUserKnob {6 enable +INVISIBLE +STARTLINE}  
enable {{parent.ILM_Kernel.Switch2}}  
addUserKnob {26 type -STARTLINE +INVISIBLE T sb.divisions}  
}  
Input {  
inputs 0  
name Input1  
xpos 5075  
ypos 1111  
}  
Dot {  
name Dot1  
xpos 5106  
ypos 1185  
}  
set Ncf91d20 [stack 0]  
Crop {  
box {{parent.parent.ILM_Kernel.Crop2.box} {parent.parent.ILM_Kernel.Crop2.box} {parent.parent.ILM_Kernel.Crop2.box} {parent.parent.ILM_Kernel.Crop2.box}}  
softness {{parent.parent.ILM_Kernel.Crop2.softness}}  
name Crop1  
xpos 4892  
ypos 1184  
}  
Dot {  
name Dot23  
xpos 4923  
ypos 1255  
}  
set Ncfa1240 [stack 0]  
Dot {  
name Dot3  
xpos 5055  
ypos 1255  
}  
Group {  
name TimeWarps  
xpos 5021  
ypos 1323  
}  
Input {  
inputs 0  
name Input  
xpos -25  
ypos -954  
}  
set Ne2777070 [stack 0]  
TimeWarp {  
lookup {{frame+0.984375}}  
length 0  
time ""  
filter none  
name TimeWarp32  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.953125}}  
length 0  
time ""  
filter none  
name TimeWarp31  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.921875}}  
length 0  
time ""  
filter none  
name TimeWarp30  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.890625}}  
length 0  
time ""  
filter none  
name TimeWarp29  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.859375}}  
length 0  
time ""  
filter none  
name TimeWarp28  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.828125}}  
length 0  
time ""  
filter none  
name TimeWarp27  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.796875}}  
length 0  
time ""  
filter none  
name TimeWarp26  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.765625}}  
length 0  
time ""  
filter none  
name TimeWarp25  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.734375}}  
length 0  
time ""  
filter none  
name TimeWarp24  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.703125}}  
length 0  
time ""  
filter none  
name TimeWarp23  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.671875}}  
length 0  
time ""  
filter none  
name TimeWarp22  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.640625}}  
length 0  
time ""  
filter none  
name TimeWarp21  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.609375}}  
length 0  
time ""  
filter none  
name TimeWarp20  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.578125}}  
length 0  
time ""  
filter none  
name TimeWarp19  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.546875}}  
length 0  
time ""  
filter none  
name TimeWarp18  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.515625}}  
length 0  
time ""  
filter none  
name TimeWarp17  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.484375}}  
length 0  
time ""  
filter none  
name TimeWarp16  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.453125}}  
length 0  
time ""  
filter none  
name TimeWarp15  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.421875}}  
length 0  
time ""  
filter none  
name TimeWarp14  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.390625}}  
length 0  
time ""  
filter none  
name TimeWarp13  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.359375}}  
length 0  
time ""  
filter none  
name TimeWarp12  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.328125}}  
length 0  
time ""  
filter none  
name TimeWarp11  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.296875}}  
length 0  
time ""  
filter none  
name TimeWarp10  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.265625}}  
length 0  
time ""  
filter none  
name TimeWarp9  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.234375}}  
length 0  
time ""  
filter none  
name TimeWarp8  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.203125}}  
length 0  
time ""  
filter none  
name TimeWarp7  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.171875}}  
length 0  
time ""  
filter none  
name TimeWarp6  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.140625}}  
length 0  
time ""  
filter none  
name TimeWarp5  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.109375}}  
length 0  
time ""  
filter none  
name TimeWarp4  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.078125}}  
length 0  
time ""  
filter none  
name TimeWarp3  
}  
push 0  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.046875}}  
length 0  
time ""  
filter none  
name TimeWarp2  
}  
push $Ne2777070  
TimeWarp {  
lookup {{frame+0.015625}}  
length 0  
time ""  
filter none  
name TimeWarp1  
}  
Merge2 {  
inputs 32+1  
also_merge all  
name Merge1  
}  
Output {  
name Output  
xpos 0  
ypos 300  
}  
end_group  
Dot {  
name Dot2  
xpos 5055  
ypos 1426  
}  
push $Ncfa1240  
TimeBlur {  
divisions {{parent.divisions}}  
shutter 0.99999  
shutteroffset end  
shuttercustomoffset 0  
name TimeBlur1  
xpos 4892  
ypos 1318  
}  
Multiply {  
value {{parent.divisions}}  
name Multiply1  
xpos 4892  
ypos 1357  
}  
Switch {  
inputs 2  
which {{parent.method}}  
name Switch_Method  
xpos 4892  
ypos 1425  
}  
Unpremult {  
channels all  
alpha sliceMatte.a  
name Unpremult2  
xpos 4892  
ypos 1511  
}  
push $Ncf91d20  
Switch {  
inputs 2  
which {{enable}}  
name Switch1  
xpos 5079  
ypos 1517  
}  
Remove {  
channels sliceMatte  
name Remove1  
xpos 5079  
ypos 1594  
}  
Output {  
name Output1  
xpos 5079  
ypos 1673  
}  
end_group  
Dot {  
name Dot47  
selected true  
xpos -1355  
ypos 12056  
}
```