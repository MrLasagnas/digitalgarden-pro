---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/crop-optimized/","tags":["nuke","node","crop","reformat"]}
---


# CropOptimized

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
    box {{"input.format.x - overscan_on*overscan.w "} {"input.format.y - overscan_on*overscan.h "} {"input.format.r + overscan_on*overscan.w "} {"input.format.t + overscan_on*overscan.h "}}
    intersect true
    name Crop_Optimized7
    knobChanged "this_knob = nuke.thisKnob()\nthis_node = nuke.thisNode()\nif this_knob.name() == \"overscan_on\":\n    this_node.knob(\"overscan\").setEnabled(this_knob.value())\n"
    label "set to input format\n\[if \{\[value overscan_on]==\"true\"\} \{return \"overscan: \[value overscan]\"\} \{return \"\"\}]"
    note_font "DejaVu Sans"
    selected true
    xpos -920
    ypos -6016
    addUserKnob {20 User}
    addUserKnob {26 text_2 l "<b>CROP TO:</b>" T " "}
    addUserKnob {22 button_settoinputformat l "input <b>FORMAT</b>" -STARTLINE T "crop_node = nuke.thisNode()\n\ncrop_node\['box'].setExpression(' input.format.x - overscan_on*overscan.w ',  0)\n\ncrop_node\['box'].setExpression(' input.format.y - overscan_on*overscan.h ',  1)\n\ncrop_node\['box'].setExpression(' input.format.r + overscan_on*overscan.w ',  2)\n\ncrop_node\['box'].setExpression(' input.format.t + overscan_on*overscan.h ',  3)\n\ncrop_node\['label'].setValue('set to input format\\n\[if \{\[value overscan_on]==\"true\"\} \{return \"overscan: \[value overscan]\"\} \{return \"\"\}]')\n\ncrop_node\['OR'].setVisible(False)\ncrop_node\['overscan_text'].setVisible(True)\ncrop_node\['overscan_on'].setVisible(True)\ncrop_node\['overscan'].setVisible(True)\n\ncrop_node\['button_bake'].setVisible(True)"}
    addUserKnob {22 button_settoinputbbox l "input <b>BBOX</b>" -STARTLINE T "crop_node = nuke.thisNode()\n\ncrop_node\['box'].setExpression(' input.bbox.x - overscan_on*overscan.w ',  0)\n\ncrop_node\['box'].setExpression(' input.bbox.y - overscan_on*overscan.h ',  1)\n\ncrop_node\['box'].setExpression(' input.bbox.r + overscan_on*overscan.w ',  2)\n\ncrop_node\['box'].setExpression(' input.bbox.t + overscan_on*overscan.h ',  3)\n\ncrop_node\['label'].setValue('set to input bbox\\n\[if \{\[value overscan_on]==\"true\"\} \{return \"overscan: \[value overscan]\"\} \{return \"\"\}]')\n\ncrop_node\['OR'].setVisible(False)\ncrop_node\['overscan_text'].setVisible(True)\ncrop_node\['overscan_on'].setVisible(True)\ncrop_node\['overscan'].setVisible(True)\n\ncrop_node\['button_bake'].setVisible(True)"}
    addUserKnob {22 button_settoratio l "custom <b>RATIO</b>" -STARTLINE T "crop_node = nuke.thisNode()\n\ncrop_node\['box'].setExpression(' (fabs(OR) < IR ? floor((IF.w-IF.h*fabs(OR))/2) : 0) - overscan_on*overscan.w ',  0)\n\ncrop_node\['box'].setExpression(' (fabs(OR) >= IR ? floor((IF.h-IF.w/fabs(OR))/2) : 0) - overscan_on*overscan.h ',  1)\n\ncrop_node\['box'].setExpression(' (fabs(OR) < IR ? ceil(IF.w-(IF.w-IF.h*fabs(OR))/2) : IF.w) + overscan_on*overscan.w ',  2)\n\ncrop_node\['box'].setExpression(' (fabs(OR) >= IR ? ceil(IF.h-(IF.h-IF.w/fabs(OR))/2) : IF.h) + overscan_on*overscan.h ',  3)\n\ncrop_node\['overscan_on'].setValue(False)\n\ncrop_node\['label'].setValue('set to ratio \[value OR]\\n\[if \{\[value overscan_on]==\"true\"\} \{return \"overscan: \[value overscan]\"\} \{return \"\"\}]')\n\ncrop_node\['OR'].setVisible(True)\ncrop_node\['overscan_text'].setVisible(True)\ncrop_node\['overscan_on'].setVisible(True)\ncrop_node\['overscan'].setVisible(True)\n\ncrop_node\['button_bake'].setVisible(True)"}
    addUserKnob {26 text l " " T " "}
    addUserKnob {26 overscan_text l <b>OVERSCAN:</b> T " "}
    addUserKnob {6 overscan_on l "" -STARTLINE}
    overscan_on true
    addUserKnob {14 overscan l "" -STARTLINE R 0 100}
    overscan 200
    addUserKnob {7 OR l <b>RATIO:</b> +HIDDEN R 0 3}
    OR 2.39
    addUserKnob {7 IR l "Input Ratio" +INVISIBLE R 0 3}
    IR {{input.format.w/input.format.h}}
    addUserKnob {14 IF l "Input Format" -STARTLINE +INVISIBLE R 0 100}
    IF {{input.width} {input.height}}
    addUserKnob {26 text_4 l " " T " "}
    addUserKnob {22 button_bake l <b>BAKE</b> T "crop_node = nuke.thisNode()\n\ncurrent_value = crop_node\['box'].value()\n\ncrop_node\['box'].setExpression('')\n\ncrop_node\['box'].setValue(current_value)\n\ncrop_node\['box'].removeKey()\n\ncrop_node\['overscan_on'].setValue(False)\n\ncrop_node\['label'].setValue('\[value box]')\n\ncrop_node\['OR'].setVisible(False)\ncrop_node\['overscan_text'].setVisible(False)\ncrop_node\['overscan_on'].setVisible(False)\ncrop_node\['overscan'].setVisible(False)\n\ncrop_node\['button_bake'].setVisible(False)" +STARTLINE}
    addUserKnob {22 button_reset l <b>RESET</b> -STARTLINE T "crop_node = nuke.thisNode()\n\ncrop_node\['box'].setExpression(' input.format.x ',  0)\ncrop_node\['box'].setExpression(' input.format.y ',  1)\ncrop_node\['box'].setExpression(' input.format.r ',  2)\ncrop_node\['box'].setExpression(' input.format.t ',  3)\n\ncurrent_value = crop_node\['box'].value()\n\ncrop_node\['box'].setExpression('')\n\ncrop_node\['box'].setValue(current_value)\n\ncrop_node\['box'].removeKey()\n\ncrop_node\['label'].setValue('\[value box]')\n\ncrop_node\['OR'].setVisible(False)\ncrop_node\['overscan_text'].setVisible(False)\ncrop_node\['overscan_on'].setVisible(False)\ncrop_node\['overscan'].setVisible(False)\n\ncrop_node\['button_bake'].setVisible(False)\n"}
    addUserKnob {26 ""}
    addUserKnob {26 text_5 l " " T philippe.desfretier@technicolor.com}
    }
```
