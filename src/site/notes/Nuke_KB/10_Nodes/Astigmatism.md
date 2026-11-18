---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/astigmatism/","tags":["nuke","node"]}
---

![[astigmatism.txt]]

## 🧠 Description


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
name Astigmatism  
knobChanged "import json\n\ndef safe_json_loads(raw_string):\n    try:\n        return json.loads(raw_string)  # Try parsing normally\n    except json.JSONDecodeError as e:\n        print(f\"Invalid JSON detected: \{repr(raw_string)\}\\nError: \{e\}\")\n        sanitized_string = raw_string.replace('\\n', '\\\\n').replace('\\t', '\\\\t')\n        try:\n            return json.loads(sanitized_string)\n        except json.JSONDecodeError:\n            print(\"Error persists after sanitation, resetting JSON.\")\n            return \{\}  # Fallback to empty dictionary\n\ndef preset_knobChanged(thisNode=None, thisKnob=None):\n    thisNode = thisNode or nuke.thisNode()\n    thisKnob = thisKnob or nuke.thisKnob()\n\n    if thisKnob.name() == 'presets':\n        if not thisNode\['_data_presets'].value():\n            nuke.tprint('No Data Stored on node')\n            return\n        \n        raw_json = thisNode\['_data_presets'].value()\n        data_dict = safe_json_loads(raw_json) if raw_json else \{\}\n\n        if thisKnob.value() in data_dict:\n            knobs_presets = data_dict\[thisKnob.value()]\n            \n            for knob_name, knob_value in knobs_presets.items():\n                if knob_name in thisNode.knobs():\n                    try:\n                        thisNode\[knob_name].fromScript(knob_value)\n                    except Exception as e:\n                        nuke.tprint(f\"Failed to set knob \{knob_name\}: \{e\}\")\n\npreset_knobChanged()\n"  
tile_color 0x6243ffff  
label "\[value presets]"  
note_font_color 0xff  
selected true  
xpos -6572  
ypos 6397  
addUserKnob {20 Astigmatism l astigmatism}  
addUserKnob {26 SetUp l "" +STARTLINE T Set-Up}  
addUserKnob {68 presets l Presets M {Default "" Alexa35/ARRI_DNA_14 Alexa35/ARRI_DNA_16 Alexa35/ARRI_DNA_21 Alexa35/ARRI_DNA_28 Alexa35/ARRI_DNA_36 Alexa35/ARRI_DNA_45 Alexa35/ARRI_DNA_50 Alexa35/ARRI_DNA_55 Alexa35/ARRI_DNA_60 Alexa35/ARRI_DNA_65 Alexa35/ARRI_DNA_80 Alexa35/ARRI_DNA_110 Alexa35/ARRI_DNA_150 Alexa35/ARRI_DNA_200 Alexa35/ARRI_HERO_35 Alexa35/ARRI_HERO_50 Alexa35/ARRI_HERO_57 Alexa35/ARRI_HERO_80 Alexa35/LAOWA_PROBE_24 Alexa35/COOKE_14 Alexa35/COOKE_16 Alexa35/COOKE_18 Alexa35/COOKE_21 Alexa35/COOKE_25 Alexa35/COOKE_35 Alexa265/ARRI_DNA_14 Alexa265/ARRI_DNA_16 Alexa265/ARRI_DNA_21 Alexa265/ARRI_DNA_28 Alexa265/ARRI_DNA_36 Alexa265/ARRI_DNA_45 Alexa265/ARRI_DNA_50 Alexa265/ARRI_DNA_55 Alexa265/ARRI_DNA_60 Alexa265/ARRI_DNA_65 Alexa265/ARRI_DNA_80 Alexa265/ARRI_DNA_110 Alexa265/ARRI_DNA_150 Alexa265/ARRI_DNA_200 Alexa265/ARRI_HERO_35 Alexa265/ARRI_HERO_50 Alexa265/ARRI_HERO_57 Alexa265/ARRI_HERO_80 Alexa265/LAOWA_PROBE_24}}  
presets Alexa265/ARRI_DNA_55  
addUserKnob {22 save_preset l "Save Preset" T "import json\n\n\nsave_knobs = \[\n    \"Tangental\",\n    \"Sagittal\",\n    \"Anamorphic\",\n    \"tan_falloff\",\n    \"inwards\",\n    \"outwards\",\n    \"redMult\",\n    \"greenMult\",\n    \"blueMult\",\n    \"magnitude\",\n    \"viewMask\",\n    \"useExtMask\",\n    \"format\",\n    \"pbb\",\n    \"format_1\",\n]\n\n\n\ndef safe_json_loads(raw_string):\n    try:\n        return json.loads(raw_string)  # Try parsing normally\n    except json.JSONDecodeError as e:\n        print(f\"Invalid JSON detected: \{repr(raw_string)\}\\nError: \{e\}\")\n        # Replace problematic characters and try again\n        sanitized_string = raw_string.replace(\"\\n\", \"\\\\n\").replace(\"\\t\", \"\\\\t\")\n        try:\n            return json.loads(sanitized_string)\n        except json.JSONDecodeError:\n            print(\"Error persists after sanitation, resetting JSON.\")\n            return \{\}  # Fallback to empty dictionary\n\n\n\ndef save_presets(thisNode=None):\n    thisNode = thisNode or nuke.thisNode()\n\n\n    raw_json = thisNode\[\"_data_presets\"].value()\n\n\n    # Try safe JSON loading\n    data_dict = safe_json_loads(raw_json) if raw_json else \{\}\n\n\n    panel = nuke.Panel(\"Save Presets\")\n    panel.addEnumerationPulldown(\n        \"preset_update\",\n        \"\\n\".join(\[\"__New__\"] + \[x.replace(\" \", \"@\") for x in list(data_dict)]),\n    )\n    panel.addSingleLineInput(\"preset_name\", \"\")\n\n\n    if not panel.show():\n        return\n    if panel.value(\"preset_update\") == \"__New__\" and not panel.value(\"preset_name\"):\n        nuke.message(\"Please write the name of the preset\")\n        return\n\n\n    if panel.value(\"preset_name\") not in \[x.replace(\" \", \"@\") for x in list(data_dict)]:\n        data_dict\[panel.value(\"preset_name\")] = \{\}\n\n\n    if panel.value(\"preset_update\") != \"__New__\":\n        prest_name = panel.value(\"preset_update\")\n    else:\n        prest_name = panel.value(\"preset_name\")\n\n\n    for knob in thisNode.allKnobs():\n        if [knob.name](http://knob.name)() in save_knobs and knob.Class() != \"Tab_Knob\" and [knob.name](http://knob.name)():\n            data_dict\[prest_name]\[[knob.name](http://knob.name)()] = knob.toScript()\n\n\n    # Ensure proper JSON encoding when saving\n    thisNode\[\"_data_presets\"].setValue(json.dumps(data_dict, ensure_ascii=False))\n    thisNode\[\"presets\"].setValues(list(data_dict))\n    thisNode\[\"presets\"].setValue(prest_name)\n\n\n\nsave_presets()\n\n" +STARTLINE}  
addUserKnob {22 delete_preset l "Delete Preset" -STARTLINE T "import json\n\ndef safe_json_loads(raw_string):\n    try:\n        return json.loads(raw_string)  # Try parsing normally\n    except json.JSONDecodeError as e:\n        print(f\"Invalid JSON detected: \{repr(raw_string)\}\\nError: \{e\}\")\n        # Replace problematic characters and try again\n        sanitized_string = raw_string.replace('\\n', '\\\\n').replace('\\t', '\\\\t')\n        try:\n            return json.loads(sanitized_string)\n        except json.JSONDecodeError:\n            print(\"Error persists after sanitation, resetting JSON.\")\n            return \{\}  # Fallback to empty dictionary\n\ndef remove_preset(thisNode=None):\n    thisNode = thisNode or nuke.thisNode()\n\n    raw_json = thisNode\['_data_presets'].value()\n    \n    # Use safe loading function\n    data_dict = safe_json_loads(raw_json) if raw_json else \{\}\n\n    if not data_dict:  # If JSON is empty or failed to parse, exit\n        return\n\n    panel = nuke.Panel('Remove Presets')\n    panel.addEnumerationPulldown('preset_name', '\\n'.join(\[x.replace(' ','@') for x in data_dict.keys()]))\n    \n    if not panel.show():\n        return\n    \n    del data_dict\[panel.value('preset_name').replace('@', ' ')]\n\n    # Ensure safe JSON storage\n    thisNode\['_data_presets'].setValue(json.dumps(data_dict, ensure_ascii=False))\n    thisNode\['presets'].setValues(list(data_dict.keys()))\n    thisNode\['presets'].setValue(0)\n\nremove_preset()\n"}  
addUserKnob {1 _data_presets +INVISIBLE}  
_data_presets "\{\"Default\": \{\"Tangental\": \"0\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"redMult\": \"1\", \"greenMult\": \"1\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"0\", \"pbb\": \"true\", \"format_1\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\"\}, \"\": \{\}, \"Alexa35/ARRI_DNA_14\": \{\"Tangental\": \"2.2\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"1\", \"greenMult\": \"1\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_DNA_16\": \{\"Tangental\": \"1.7\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"1\", \"greenMult\": \"1\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_DNA_21\": \{\"Tangental\": \"1.8\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"1\", \"greenMult\": \"1\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_DNA_28\": \{\"Tangental\": \"2.7\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.475\", \"greenMult\": \"0.785\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_DNA_36\": \{\"Tangental\": \"3.2\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.475\", \"greenMult\": \"0.785\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_DNA_45\": \{\"Tangental\": \"3.2\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.475\", \"greenMult\": \"0.785\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_DNA_50\": \{\"Tangental\": \"3.2\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.475\", \"greenMult\": \"0.785\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_DNA_55\": \{\"Tangental\": \"2.7\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.725\", \"greenMult\": \"0.865\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_DNA_60\": \{\"Tangental\": \"1.8\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.81\", \"greenMult\": \"0.92\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_DNA_65\": \{\"Tangental\": \"1.2\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.81\", \"greenMult\": \"0.92\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_DNA_80\": \{\"Tangental\": \"1.2\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.64\", \"greenMult\": \"0.83\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_DNA_110\": \{\"Tangental\": \"1\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.64\", \"greenMult\": \"0.83\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_DNA_150\": \{\"Tangental\": \"1.3\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.64\", \"greenMult\": \"0.83\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_DNA_200\": \{\"Tangental\": \"1.3\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.64\", \"greenMult\": \"0.83\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_HERO_35\": \{\"Tangental\": \"3.2\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.64\", \"greenMult\": \"0.83\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_HERO_50\": \{\"Tangental\": \"5.2\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.31\", \"greenMult\": \"0.65\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_HERO_57\": \{\"Tangental\": \"4\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.31\", \"greenMult\": \"0.65\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/ARRI_HERO_80\": \{\"Tangental\": \"4.4\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.31\", \"greenMult\": \"0.65\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/LAOWA_PROBE_24\": \{\"Tangental\": \"1.1\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.72\", \"greenMult\": \"0.87\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/COOKE_14\": \{\"Tangental\": \"2.6\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.215\", \"greenMult\": \"0.545\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/COOKE_16\": \{\"Tangental\": \"2.3\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.215\", \"greenMult\": \"0.545\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/COOKE_18\": \{\"Tangental\": \"2.3\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.215\", \"greenMult\": \"0.545\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/COOKE_21\": \{\"Tangental\": \"2.3\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.215\", \"greenMult\": \"0.545\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/COOKE_25\": \{\"Tangental\": \"1.6\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.215\", \"greenMult\": \"0.545\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa35/COOKE_35\": \{\"Tangental\": \"1.3\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.215\", \"greenMult\": \"0.545\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2148 1476 0 0 2148 1476 1 alexa_35_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa265/ARRI_DNA_14\": \{\"Tangental\": \"3\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"1\", \"greenMult\": \"1\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa265/ARRI_DNA_16\": \{\"Tangental\": \"1.7\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"1\", \"greenMult\": \"1\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa265/ARRI_DNA_21\": \{\"Tangental\": \"1.8\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"1\", \"greenMult\": \"1\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa265/ARRI_DNA_28\": \{\"Tangental\": \"2.7\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.475\", \"greenMult\": \"0.785\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa265/ARRI_DNA_36\": \{\"Tangental\": \"2.7\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.475\", \"greenMult\": \"0.785\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa265/ARRI_DNA_45\": \{\"Tangental\": \"3.2\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.475\", \"greenMult\": \"0.785\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa265/ARRI_DNA_50\": \{\"Tangental\": \"3.2\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.475\", \"greenMult\": \"0.785\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa265/ARRI_DNA_55\": \{\"Tangental\": \"2.7\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.725\", \"greenMult\": \"0.865\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa265/ARRI_DNA_60\": \{\"Tangental\": \"1.8\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.81\", \"greenMult\": \"0.92\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa265/ARRI_DNA_65\": \{\"Tangental\": \"1.2\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.81\", \"greenMult\": \"0.92\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa265/ARRI_DNA_80\": \{\"Tangental\": \"1.2\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.64\", \"greenMult\": \"0.83\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa265/ARRI_DNA_110\": \{\"Tangental\": \"1\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.64\", \"greenMult\": \"0.83\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa265/ARRI_DNA_150\": \{\"Tangental\": \"1.3\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.64\", \"greenMult\": \"0.83\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa265/ARRI_DNA_200\": \{\"Tangental\": \"1.3\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.64\", \"greenMult\": \"0.83\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa265/ARRI_HERO_35\": \{\"Tangental\": \"3.2\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.64\", \"greenMult\": \"0.83\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\", \"tan_falloff\": \"linear\"\}, \"Alexa265/ARRI_HERO_50\": \{\"Tangental\": \"4.6\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"tan_falloff\": \"linear\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.89\", \"greenMult\": \"0.96\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\"\}, \"Alexa265/ARRI_HERO_57\": \{\"Tangental\": \"6.2\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"tan_falloff\": \"linear\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.89\", \"greenMult\": \"0.96\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\"\}, \"Alexa265/ARRI_HERO_80\": \{\"Tangental\": \"11\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"tan_falloff\": \"linear\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.89\", \"greenMult\": \"0.96\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\"\}, \"Alexa265/LAOWA_PROBE_24\": \{\"Tangental\": \"1\", \"Sagittal\": \"0\", \"Anamorphic\": \"0\", \"tan_falloff\": \"linear\", \"inwards\": \"1\", \"outwards\": \"1\", \"redMult\": \"0.89\", \"greenMult\": \"0.96\", \"blueMult\": \"1\", \"magnitude\": \"1\", \"viewMask\": \"false\", \"useExtMask\": \"true\", \"format\": \"2458 1162 0 0 2458 1162 1 alexa_265_opengate_sph_ext93\", \"pbb\": \"true\", \"format_1\": \"0\"\}\}"  
addUserKnob {26 ""}  
addUserKnob {14 Tangental l tangental R 0 100}  
Tangental 5  
addUserKnob {7 Sagittal l sagittal R 0 0.5}  
addUserKnob {7 Anamorphic l anamorphic}  
addUserKnob {26 ""}  
addUserKnob {4 tan_falloff l "tangential falloff" M {linear smooth}}  
addUserKnob {26 weight l "" +STARTLINE T "tangental weight"}  
addUserKnob {41 inwards T Inwards.mix}  
addUserKnob {41 outwards T Outwards.mix}  
addUserKnob {26 ""}  
addUserKnob {26 channelContribution l "" +STARTLINE T "channel contribution"}  
addUserKnob {41 redMult l red T RedMult.mix}  
addUserKnob {41 greenMult l green T GreenMult.mix}  
addUserKnob {41 blueMult l blue T BlueMult.mix}  
addUserKnob {26 ""}  
addUserKnob {26 magnitudeMask l "" +STARTLINE T "magnitude mask"}  
addUserKnob {41 magnitude T Magnitude.mix}  
addUserKnob {6 viewMask l "view magnitude mask" +STARTLINE}  
addUserKnob {6 useExtMask l "use external magnitude mask if exists" +STARTLINE}  
useExtMask true  
addUserKnob {26 ""}  
addUserKnob {41 format l "plate format" T Reformat1.format}  
addUserKnob {41 pbb l "preserve bounding box" -STARTLINE T Reformat1.pbb}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
addUserKnob {26 disco_node_index l disco_node_index: +INVISIBLE T "1 / 1"}  
}  
BackdropNode {  
inputs 0  
name BackdropNode1  
label "if step = 5 blur= root*20%"  
xpos 231  
ypos 288  
bdwidth 240  
bdheight 214  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name BackdropNode10  
label "if step = 5 blur= root*100%"  
xpos 2436  
ypos 1596  
bdwidth 240  
bdheight 214  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name BackdropNode11  
label "if step = 5 blur= root*20%"  
xpos 4391  
ypos 288  
bdwidth 240  
bdheight 214  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name BackdropNode12  
label "if step = 5 blur= root*40%"  
xpos 4459  
ypos 606  
bdwidth 240  
bdheight 214  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name BackdropNode13  
label "if step = 5 blur= root*60%"  
xpos 4512  
ypos 954  
bdwidth 240  
bdheight 214  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name BackdropNode14  
label "if step = 5 blur= root*80%"  
xpos 4569  
ypos 1274  
bdwidth 240  
bdheight 214  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name BackdropNode15  
label "if step = 5 blur= root*100%"  
xpos 4586  
ypos 1596  
bdwidth 240  
bdheight 214  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name BackdropNode2  
label "if step = 5 blur= root*40%"  
xpos 299  
ypos 606  
bdwidth 240  
bdheight 214  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name BackdropNode3  
label "if step = 5 blur= root*60%"  
xpos 352  
ypos 954  
bdwidth 240  
bdheight 214  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name BackdropNode4  
label "if step = 5 blur= root*80%"  
xpos 409  
ypos 1274  
bdwidth 240  
bdheight 214  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name BackdropNode5  
label "if step = 5 blur= root*100%"  
xpos 426  
ypos 1596  
bdwidth 240  
bdheight 214  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name BackdropNode6  
label "if step = 5 blur= root*20%"  
xpos 2241  
ypos 288  
bdwidth 240  
bdheight 214  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name BackdropNode7  
label "if step = 5 blur= root*40%"  
xpos 2309  
ypos 606  
bdwidth 240  
bdheight 214  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name BackdropNode8  
label "if step = 5 blur= root*60%"  
xpos 2362  
ypos 954  
bdwidth 240  
bdheight 214  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name BackdropNode9  
label "if step = 5 blur= root*80%"  
xpos 2419  
ypos 1274  
bdwidth 240  
bdheight 214  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice1  
label "lo:0.0, hi:0.2 gradient\ncentre: 0.1"  
xpos 861  
ypos 174  
bdwidth 134  
bdheight 154  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice10  
label "lo: 1, hi: 1.2\nCentre: 1.1"  
xpos 864  
ypos 1643  
bdwidth 140  
bdheight 166  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice11  
label "lo:0.0, hi:0.2 gradient\ncentre: 0.1"  
xpos 2871  
ypos 174  
bdwidth 134  
bdheight 154  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice12  
label "lo: 1, hi: 1.2\nCentre: 1.1"  
xpos 2874  
ypos 1643  
bdwidth 140  
bdheight 166  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice13  
label "lo: 0.2, hi: 0.4\nCentre: 0.3"  
xpos 2872  
ypos 332  
bdwidth 137  
bdheight 159  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice14  
label "lo: 0.2, hi: 0.4 + Gradient\nCentre: 0.3"  
xpos 2870  
ypos 498  
bdwidth 142  
bdheight 160  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice15  
label "lo: 0.4, hi: 0.6\nCentre: 0.5"  
xpos 2872  
ypos 665  
bdwidth 138  
bdheight 156  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice16  
label "lo: 0.4, hi: 0.6 + Gradient\nCentre: 0.5"  
xpos 2873  
ypos 827  
bdwidth 138  
bdheight 152  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice17  
label "lo: 0.6, hi: 0.8\nCentre: 0.7"  
xpos 2875  
ypos 987  
bdwidth 134  
bdheight 158  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice18  
label "lo: 0.6, hi: 0.8 + Gradient\nCentre: 0.7"  
xpos 2873  
ypos 1150  
bdwidth 138  
bdheight 154  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice19  
label "lo: 0.8, hi: 1.0\nCentre: 0.9"  
xpos 2873  
ypos 1311  
bdwidth 140  
bdheight 162  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice2  
label "lo: 0.2, hi: 0.4\nCentre: 0.3"  
xpos 862  
ypos 332  
bdwidth 137  
bdheight 159  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice20  
label "lo: 0.8, hi: 1.0 + Gradient\nCentre: 0.9"  
xpos 2873  
ypos 1483  
bdwidth 135  
bdheight 154  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice21  
label "lo:0.0, hi:0.2 gradient\ncentre: 0.1"  
xpos 5021  
ypos 174  
bdwidth 134  
bdheight 154  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice22  
label "lo: 1, hi: 1.2\nCentre: 1.1"  
xpos 5024  
ypos 1643  
bdwidth 140  
bdheight 166  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice23  
label "lo: 0.2, hi: 0.4\nCentre: 0.3"  
xpos 5022  
ypos 332  
bdwidth 137  
bdheight 159  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice24  
label "lo: 0.2, hi: 0.4 + Gradient\nCentre: 0.3"  
xpos 5020  
ypos 498  
bdwidth 142  
bdheight 160  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice25  
label "lo: 0.4, hi: 0.6\nCentre: 0.5"  
xpos 5022  
ypos 665  
bdwidth 138  
bdheight 156  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice26  
label "lo: 0.4, hi: 0.6 + Gradient\nCentre: 0.5"  
xpos 5023  
ypos 827  
bdwidth 138  
bdheight 152  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice27  
label "lo: 0.6, hi: 0.8\nCentre: 0.7"  
xpos 5025  
ypos 987  
bdwidth 134  
bdheight 158  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice28  
label "lo: 0.6, hi: 0.8 + Gradient\nCentre: 0.7"  
xpos 5023  
ypos 1150  
bdwidth 138  
bdheight 154  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice29  
label "lo: 0.8, hi: 1.0\nCentre: 0.9"  
xpos 5023  
ypos 1311  
bdwidth 140  
bdheight 162  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice3  
label "lo: 0.2, hi: 0.4 + Gradient\nCentre: 0.3"  
xpos 860  
ypos 498  
bdwidth 142  
bdheight 160  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice30  
label "lo: 0.8, hi: 1.0 + Gradient\nCentre: 0.9"  
xpos 5023  
ypos 1483  
bdwidth 135  
bdheight 154  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice4  
label "lo: 0.4, hi: 0.6\nCentre: 0.5"  
xpos 862  
ypos 665  
bdwidth 138  
bdheight 156  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice5  
label "lo: 0.4, hi: 0.6 + Gradient\nCentre: 0.5"  
xpos 863  
ypos 827  
bdwidth 138  
bdheight 152  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice6  
label "lo: 0.6, hi: 0.8\nCentre: 0.7"  
xpos 865  
ypos 987  
bdwidth 134  
bdheight 158  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice7  
label "lo: 0.6, hi: 0.8 + Gradient\nCentre: 0.7"  
xpos 863  
ypos 1150  
bdwidth 138  
bdheight 154  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice8  
label "lo: 0.8, hi: 1.0\nCentre: 0.9"  
xpos 863  
ypos 1311  
bdwidth 140  
bdheight 162  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
BackdropNode {  
inputs 0  
name DepthSlice9  
label "lo: 0.8, hi: 1.0 + Gradient\nCentre: 0.9"  
xpos 863  
ypos 1483  
bdwidth 135  
bdheight 154  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Reformat {  
inputs 0  
format "4096 4096 0 0 4096 4096 1 square_4k"  
name Reformat3  
xpos 6134  
ypos -1248  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Radial {  
area {0 0 4096 4096}  
softness {{(Magnitude.mix)}}  
name Radial1  
xpos 6134  
ypos -1210  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
white {{1-int(Magnitude.mix)}}  
gamma {{sqrt(1-Magnitude.mix)}}  
white_clamp true  
name Grade31  
xpos 6134  
ypos -1184  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Transform {  
scale 1.41541  
center {2048 2048}  
shutteroffset centred  
name Transform1  
xpos 6134  
ypos -1158  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Reformat {  
name Reformat4  
xpos 6134  
ypos -1132  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Invert {  
name Invert1  
xpos 6134  
ypos -1106  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Dot {  
name Dot27  
xpos 6168  
ypos -961  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N9d459060 [stack 0]  
Dot {  
name Dot28  
xpos 6168  
ypos 2550  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Input {  
inputs 0  
name mask  
xpos 5604  
ypos -1148  
number 1  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N9d459060  
Switch {  
inputs 2  
which {{"\[exists parent.input1] & parent.useExtMask"}}  
name Switch4  
xpos 5604  
ypos -965  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Reformat {  
format "4916 2322 0 0 4916 2322 1 A265_6560x3100_PA10_93 Full Frame VFX WR"  
resize none  
pbb true  
name Reformat1  
xpos 5604  
ypos -898  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set Ccaafe4f0 [stack 0]  
Shuffle {  
red black  
green black  
blue black  
alpha black  
black alpha  
out2 depth  
name Shuffle3  
xpos 5604  
ypos -820  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Dot {  
name Dot29  
xpos 5638  
ypos -693  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set Nef0975b0 [stack 0]  
Dot {  
name Dot15  
xpos 5638  
ypos 233  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N3a0bb340 [stack 0]  
ZSlice {  
Zchan depth.Z  
center 1.1  
dof 0.2  
name ZSlice21  
xpos 5045  
ypos 1692  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle22  
xpos 5045  
ypos 1735  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.9  
white_clamp true  
name Grade21  
xpos 5045  
ypos 1764  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Input {  
inputs 0  
name Input  
xpos -512  
ypos -483  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
clone $Ccaafe4f0 {  
xpos -514  
ypos -429  
selected false  
}  
Dot {  
name Dot22  
xpos -478  
ypos -20  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N13a30db0 [stack 0]  
Dot {  
name Dot4  
xpos 30  
ypos -20  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N58cf1190 [stack 0]  
Dot {  
name Dot8  
xpos 601  
ypos -20  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set Nc25acb40 [stack 0]  
Dot {  
name Dot9  
xpos 2044  
ypos -20  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set Nac47ca40 [stack 0]  
Dot {  
name Dot10  
xpos 2611  
ypos -20  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N4eb585d0 [stack 0]  
Dot {  
name Dot16  
xpos 4194  
ypos -20  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set Nef005990 [stack 0]  
Dot {  
name Dot17  
xpos 4761  
ypos -20  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Dot {  
name Dot18  
xpos 4761  
ypos 94  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N8cec6d40 [stack 0]  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*BlueMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*BlueMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays27  
xpos 4739  
ypos 1488  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays35.scale==1 ? 0 : tan_falloff"}}  
name Multiply36  
xpos 4739  
ypos 1514  
disable {{"GodRays35.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*BlueMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*BlueMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays28  
xpos 4739  
ypos 1540  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays36.scale==1 ? 0 : tan_falloff"}}  
name Multiply35  
xpos 4739  
ypos 1566  
disable {{"GodRays36.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays107  
xpos 4739  
ypos 1592  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays108  
xpos 4739  
ypos 1618  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{parent.Anamorphic/10*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays109  
xpos 4739  
ypos 1655  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-parent.Anamorphic/10*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays110  
xpos 4739  
ypos 1681  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N3a2bb3d0 [stack 0]  
push $N3a0bb340  
ZSlice {  
Zchan depth.Z  
center 0.9  
dof 0.2  
name ZSlice22  
xpos 5044  
ypos 1535  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle23  
xpos 5044  
ypos 1578  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.8  
white_clamp true  
name Grade22  
xpos 5044  
ypos 1607  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N3a2bb3d0  
push $N3a0bb340  
ZSlice {  
Zchan depth.Z  
center 0.9  
dof 0.2  
name ZSlice23  
xpos 5047  
ypos 1361  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle24  
xpos 5047  
ypos 1404  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.79  
whitepoint 0.8  
white_clamp true  
name Grade23  
xpos 5047  
ypos 1433  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N8cec6d40  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*.8*BlueMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*.8*BlueMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays101  
xpos 4716  
ypos 1123  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays35.scale==1 ? 0 : tan_falloff"}}  
name Multiply34  
xpos 4716  
ypos 1149  
disable {{"GodRays35.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*.8*BlueMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*.8*BlueMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays102  
xpos 4716  
ypos 1175  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays36.scale==1 ? 0 : tan_falloff"}}  
name Multiply33  
xpos 4716  
ypos 1201  
disable {{"GodRays36.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*.8*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays103  
xpos 4716  
ypos 1234  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*.8*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays104  
xpos 4716  
ypos 1260  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{parent.Anamorphic/10*.8*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays105  
xpos 4716  
ypos 1297  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-parent.Anamorphic/10*.8*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays106  
xpos 4716  
ypos 1323  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N38eb9a40 [stack 0]  
push $N3a0bb340  
ZSlice {  
Zchan depth.Z  
center 0.7  
dof 0.2  
name ZSlice24  
xpos 5054  
ypos 1200  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle25  
xpos 5054  
ypos 1243  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.6  
whitepoint 0.8  
white_clamp true  
name Grade24  
xpos 5054  
ypos 1272  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N38eb9a40  
push $N3a0bb340  
ZSlice {  
Zchan depth.Z  
center 0.7  
dof 0.2  
name ZSlice25  
xpos 5047  
ypos 1034  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle26  
xpos 5047  
ypos 1077  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.59  
whitepoint 0.6  
white_clamp true  
name Grade25  
xpos 5047  
ypos 1106  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N8cec6d40  
Dot {  
name Dot19  
xpos 4705  
ypos 778  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*.6*BlueMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*.6*BlueMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays29  
xpos 4671  
ypos 808  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays35.scale==1 ? 0 : tan_falloff"}}  
name Multiply32  
xpos 4671  
ypos 834  
disable {{"GodRays35.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*.6*BlueMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*.6*BlueMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays30  
xpos 4671  
ypos 860  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays36.scale==1 ? 0 : tan_falloff"}}  
name Multiply31  
xpos 4671  
ypos 886  
disable {{"GodRays36.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*.6*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays97  
xpos 4671  
ypos 920  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*.6*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays98  
xpos 4671  
ypos 946  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{parent.Anamorphic/10*.6*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays99  
xpos 4671  
ypos 983  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-parent.Anamorphic/10*.6*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays100  
xpos 4671  
ypos 1009  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N563fc870 [stack 0]  
push $N3a0bb340  
ZSlice {  
Zchan depth.Z  
dof 0.2  
name ZSlice26  
xpos 5050  
ypos 877  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle27  
xpos 5050  
ypos 920  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.4  
whitepoint 0.6  
white_clamp true  
name Grade26  
xpos 5050  
ypos 949  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N563fc870  
push $N3a0bb340  
ZSlice {  
Zchan depth.Z  
dof 0.2  
name ZSlice27  
xpos 5040  
ypos 715  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle28  
xpos 5040  
ypos 758  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.39  
whitepoint 0.4  
white_clamp true  
name Grade27  
xpos 5040  
ypos 787  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N8cec6d40  
Dot {  
name Dot20  
xpos 4657  
ypos 431  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*.4*BlueMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*.4*BlueMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays31  
xpos 4623  
ypos 468  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays35.scale==1 ? 0 : tan_falloff"}}  
name Multiply30  
xpos 4623  
ypos 494  
disable {{"GodRays35.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*.4*BlueMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*.4*BlueMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays32  
xpos 4623  
ypos 520  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays36.scale==1 ? 0 : tan_falloff"}}  
name Multiply29  
xpos 4623  
ypos 546  
disable {{"GodRays36.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*.4*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays33  
xpos 4623  
ypos 586  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*.4*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays34  
xpos 4623  
ypos 612  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{parent.Anamorphic/10*.4*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays95  
xpos 4623  
ypos 649  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-parent.Anamorphic/10*.4*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays96  
xpos 4623  
ypos 675  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N30d76980 [stack 0]  
push $N3a0bb340  
ZSlice {  
Zchan depth.Z  
center 0.3  
dof 0.2  
name ZSlice28  
xpos 5044  
ypos 550  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle29  
xpos 5044  
ypos 593  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.2  
whitepoint 0.4  
white_clamp true  
name Grade28  
xpos 5044  
ypos 619  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N30d76980  
push $N3a0bb340  
ZSlice {  
Zchan depth.Z  
center 0.3  
dof 0.2  
name ZSlice29  
xpos 5035  
ypos 383  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle30  
xpos 5035  
ypos 426  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.19  
whitepoint 0.2  
white_clamp true  
name Grade29  
xpos 5035  
ypos 455  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N8cec6d40  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*.2*BlueMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*.2*BlueMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays89  
xpos 4564  
ypos 140  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays35.scale==1 ? 0 : tan_falloff"}}  
name Multiply28  
xpos 4564  
ypos 166  
disable {{"GodRays35.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*.2*BlueMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*.2*BlueMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays90  
xpos 4564  
ypos 192  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays36.scale==1 ? 0 : tan_falloff"}}  
name Multiply27  
xpos 4564  
ypos 218  
disable {{"GodRays36.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*.2*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays91  
xpos 4564  
ypos 278  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*.2*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays92  
xpos 4564  
ypos 304  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{parent.Anamorphic/10*.2*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays93  
xpos 4564  
ypos 341  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-parent.Anamorphic/10*.2*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays94  
xpos 4564  
ypos 367  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N7077e810 [stack 0]  
push $N3a0bb340  
ZSlice {  
Zchan depth.Z  
center 0.1  
dof 0.2  
name ZSlice30  
xpos 5033  
ypos 223  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle31  
xpos 5033  
ypos 266  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
whitepoint 0.2  
white_clamp true  
name Grade30  
xpos 5033  
ypos 295  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N7077e810  
push $N8cec6d40  
Keymix {  
inputs 3  
name Keymix21  
xpos 4422  
ypos 391  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix22  
xpos 4487  
ypos 449  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix23  
xpos 4487  
ypos 700  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix24  
xpos 4549  
ypos 781  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix25  
xpos 4549  
ypos 1028  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix26  
xpos 4589  
ypos 1100  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix27  
xpos 4589  
ypos 1335  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix28  
xpos 4618  
ypos 1427  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix29  
xpos 4618  
ypos 1685  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix30  
xpos 4631  
ypos 1758  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Dot {  
name Dot21  
xpos 4665  
ypos 1881  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $Nef005990  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*BlueMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*BlueMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays35  
xpos 4160  
ypos 137  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays35.scale==1 ? 0 : tan_falloff"}}  
name Multiply26  
xpos 4160  
ypos 167  
disable {{"GodRays35.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*BlueMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*BlueMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays36  
xpos 4160  
ypos 193  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays36.scale==1 ? 0 : tan_falloff"}}  
name Multiply25  
xpos 4160  
ypos 219  
disable {{"GodRays36.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays13  
xpos 4160  
ypos 295  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays14  
xpos 4160  
ypos 321  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{parent.Anamorphic/10*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays87  
xpos 4160  
ypos 358  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-parent.Anamorphic/10*BlueMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays88  
xpos 4160  
ypos 384  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Switch {  
inputs 2  
which {{"\[exists parent.input1] & parent.useExtMask | !(parent.magnitude==1)"}}  
name Switch2  
xpos 4160  
ypos 1877  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Dot {  
name Dot25  
xpos 4194  
ypos 2216  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set Ne44b1d40 [stack 0]  
push 0  
push $Nef0975b0  
Dot {  
name Dot7  
xpos 3488  
ypos 233  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N9c6fbba0 [stack 0]  
ZSlice {  
Zchan depth.Z  
center 1.1  
dof 0.2  
name ZSlice3  
xpos 2895  
ypos 1692  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle1  
xpos 2895  
ypos 1735  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.9  
white_clamp true  
name Grade1  
xpos 2895  
ypos 1764  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N4eb585d0  
Dot {  
name Dot11  
xpos 2611  
ypos 94  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N7327bc10 [stack 0]  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*GreenMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*GreenMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays81  
xpos 2602  
ypos 1480  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays23.scale==1 ? 0 : tan_falloff"}}  
name Multiply24  
xpos 2602  
ypos 1506  
disable {{"GodRays23.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*GreenMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*GreenMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays82  
xpos 2602  
ypos 1535  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays24.scale==1 ? 0 : tan_falloff"}}  
name Multiply23  
xpos 2602  
ypos 1561  
disable {{"GodRays24.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays83  
xpos 2602  
ypos 1592  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays84  
xpos 2602  
ypos 1618  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{(parent.Anamorphic/10)*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays85  
xpos 2602  
ypos 1650  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-(parent.Anamorphic/10)*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays86  
xpos 2602  
ypos 1676  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set Nd8864f0 [stack 0]  
push $N9c6fbba0  
ZSlice {  
Zchan depth.Z  
center 0.9  
dof 0.2  
name ZSlice4  
xpos 2894  
ypos 1535  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle2  
xpos 2894  
ypos 1578  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.8  
white_clamp true  
name Grade12  
xpos 2894  
ypos 1607  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $Nd8864f0  
push $N9c6fbba0  
ZSlice {  
Zchan depth.Z  
center 0.9  
dof 0.2  
name ZSlice5  
xpos 2897  
ypos 1361  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle4  
xpos 2897  
ypos 1404  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.79  
whitepoint 0.8  
white_clamp true  
name Grade13  
xpos 2897  
ypos 1433  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N7327bc10  
Dot {  
name Dot13  
xpos 2581  
ypos 1070  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*.8*GreenMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*.8*GreenMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays15  
xpos 2547  
ypos 1112  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays23.scale==1 ? 0 : tan_falloff"}}  
name Multiply22  
xpos 2547  
ypos 1138  
disable {{"GodRays23.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*.8*GreenMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*.8*GreenMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays16  
xpos 2547  
ypos 1164  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays24.scale==1 ? 0 : tan_falloff"}}  
name Multiply21  
xpos 2547  
ypos 1190  
disable {{"GodRays24.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*.8*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays77  
xpos 2547  
ypos 1232  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*.8*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays78  
xpos 2547  
ypos 1258  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{(parent.Anamorphic/10)*.8*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays79  
xpos 2547  
ypos 1290  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-(parent.Anamorphic/10)*.8*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays80  
xpos 2547  
ypos 1316  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set Ne5bb2720 [stack 0]  
push $N9c6fbba0  
ZSlice {  
Zchan depth.Z  
center 0.7  
dof 0.2  
name ZSlice6  
xpos 2904  
ypos 1200  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle5  
xpos 2904  
ypos 1243  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.6  
whitepoint 0.8  
white_clamp true  
name Grade14  
xpos 2904  
ypos 1272  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $Ne5bb2720  
push $N9c6fbba0  
ZSlice {  
Zchan depth.Z  
center 0.7  
dof 0.2  
name ZSlice15  
xpos 2897  
ypos 1034  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle6  
xpos 2897  
ypos 1077  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.59  
whitepoint 0.6  
white_clamp true  
name Grade15  
xpos 2897  
ypos 1106  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N7327bc10  
Dot {  
name Dot12  
xpos 2553  
ypos 747  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*.6*GreenMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*.6*GreenMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays17  
xpos 2519  
ypos 780  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays23.scale==1 ? 0 : tan_falloff"}}  
name Multiply20  
xpos 2519  
ypos 806  
disable {{"GodRays23.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*.6*GreenMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*.6*GreenMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays18  
xpos 2519  
ypos 832  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays24.scale==1 ? 0 : tan_falloff"}}  
name Multiply19  
xpos 2519  
ypos 858  
disable {{"GodRays24.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*.6*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays73  
xpos 2519  
ypos 907  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*.6*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays74  
xpos 2519  
ypos 933  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{(parent.Anamorphic/10)*.6*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays75  
xpos 2519  
ypos 965  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-(parent.Anamorphic/10)*.6*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays76  
xpos 2519  
ypos 991  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N547f6fd0 [stack 0]  
push $N9c6fbba0  
ZSlice {  
Zchan depth.Z  
dof 0.2  
name ZSlice16  
xpos 2900  
ypos 877  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle8  
xpos 2900  
ypos 920  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.4  
whitepoint 0.6  
white_clamp true  
name Grade16  
xpos 2900  
ypos 949  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N547f6fd0  
push $N9c6fbba0  
ZSlice {  
Zchan depth.Z  
dof 0.2  
name ZSlice17  
xpos 2890  
ypos 715  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle10  
xpos 2890  
ypos 758  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.39  
whitepoint 0.4  
white_clamp true  
name Grade17  
xpos 2890  
ypos 787  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N7327bc10  
Dot {  
name Dot33  
xpos 2499  
ypos 466  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*.4*GreenMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*.4*GreenMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays19  
xpos 2465  
ypos 484  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays23.scale==1 ? 0 : tan_falloff"}}  
name Multiply18  
xpos 2465  
ypos 510  
disable {{"GodRays23.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*.4*GreenMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*.4*GreenMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays20  
xpos 2465  
ypos 536  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays24.scale==1 ? 0 : tan_falloff"}}  
name Multiply17  
xpos 2465  
ypos 564  
disable {{"GodRays24.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*.4*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays69  
xpos 2465  
ypos 592  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*.4*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays70  
xpos 2465  
ypos 618  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{(parent.Anamorphic/10)*.4*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays71  
xpos 2465  
ypos 650  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-(parent.Anamorphic/10)*.4*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays72  
xpos 2465  
ypos 676  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N3a692d90 [stack 0]  
push $N9c6fbba0  
ZSlice {  
Zchan depth.Z  
center 0.3  
dof 0.2  
name ZSlice18  
xpos 2894  
ypos 550  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle19  
xpos 2894  
ypos 593  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.2  
whitepoint 0.4  
white_clamp true  
name Grade18  
xpos 2894  
ypos 619  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N3a692d90  
push $N9c6fbba0  
ZSlice {  
Zchan depth.Z  
center 0.3  
dof 0.2  
name ZSlice19  
xpos 2885  
ypos 383  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle20  
xpos 2885  
ypos 426  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.19  
whitepoint 0.2  
white_clamp true  
name Grade19  
xpos 2885  
ypos 455  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N7327bc10  
Dot {  
name Dot32  
xpos 2429  
ypos 147  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*.2*GreenMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*.2*GreenMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays21  
xpos 2395  
ypos 173  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays23.scale==1 ? 0 : tan_falloff"}}  
name Multiply16  
xpos 2395  
ypos 199  
disable {{"GodRays23.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*.2*GreenMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*.2*GreenMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays22  
xpos 2395  
ypos 225  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays24.scale==1 ? 0 : tan_falloff"}}  
name Multiply15  
xpos 2395  
ypos 251  
disable {{"GodRays24.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*.2*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays65  
xpos 2395  
ypos 313  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*.2*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays66  
xpos 2395  
ypos 339  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{(parent.Anamorphic/10)*.2*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays67  
xpos 2395  
ypos 371  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-(parent.Anamorphic/10)*.2*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays68  
xpos 2395  
ypos 397  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set Nde2ecc0 [stack 0]  
push $N9c6fbba0  
ZSlice {  
Zchan depth.Z  
center 0.1  
dof 0.2  
name ZSlice20  
xpos 2883  
ypos 223  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle21  
xpos 2883  
ypos 266  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
whitepoint 0.2  
white_clamp true  
name Grade20  
xpos 2883  
ypos 292  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $Nde2ecc0  
push $N7327bc10  
Keymix {  
inputs 3  
name Keymix11  
xpos 2272  
ypos 391  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix12  
xpos 2337  
ypos 449  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix13  
xpos 2337  
ypos 700  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix14  
xpos 2399  
ypos 781  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix15  
xpos 2399  
ypos 1028  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix16  
xpos 2439  
ypos 1100  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix17  
xpos 2439  
ypos 1335  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix18  
xpos 2468  
ypos 1427  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix19  
xpos 2468  
ypos 1685  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix20  
xpos 2481  
ypos 1758  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Dot {  
name Dot14  
xpos 2515  
ypos 1881  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $Nac47ca40  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*GreenMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*GreenMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays23  
xpos 2010  
ypos 121  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays23.scale==1 ? 0 : tan_falloff"}}  
name Multiply13  
xpos 2010  
ypos 147  
disable {{"GodRays23.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*GreenMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*GreenMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays24  
xpos 2010  
ypos 203  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays24.scale==1 ? 0 : tan_falloff"}}  
name Multiply14  
xpos 2010  
ypos 229  
disable {{"GodRays24.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays11  
xpos 2010  
ypos 291  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays12  
xpos 2010  
ypos 317  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{parent.Anamorphic/10*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays63  
xpos 2010  
ypos 361  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-parent.Anamorphic/10*GreenMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays64  
xpos 2010  
ypos 387  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Switch {  
inputs 2  
which {{"\[exists parent.input1] & parent.useExtMask | !(parent.magnitude==1)"}}  
name Switch1  
xpos 2010  
ypos 1877  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Dot {  
name Dot24  
xpos 2044  
ypos 2178  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set Nfd579d40 [stack 0]  
push $Nef0975b0  
Dot {  
name Dot6  
xpos 1478  
ypos 233  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set Nfd3744c0 [stack 0]  
ZSlice {  
Zchan depth.Z  
center 1.1  
dof 0.2  
name ZSlice14  
xpos 885  
ypos 1692  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle18  
xpos 885  
ypos 1735  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.9  
white_clamp true  
name Grade11  
xpos 885  
ypos 1764  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $Nc25acb40  
Dot {  
name Dot2  
xpos 601  
ypos 93  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set Nfcb7d220 [stack 0]  
Dot {  
name Dot31  
xpos 601  
ypos 1407  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*RedMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*RedMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays9  
xpos 567  
ypos 1442  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays1.scale==1 ? 0 : tan_falloff"}}  
name Multiply11  
xpos 567  
ypos 1468  
disable {{"GodRays1.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*RedMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*RedMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays10  
xpos 567  
ypos 1494  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays2.scale==1 ? 0 : tan_falloff"}}  
name Multiply12  
xpos 567  
ypos 1520  
disable {{"GodRays2.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays59  
xpos 567  
ypos 1546  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays60  
xpos 567  
ypos 1572  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{(parent.Anamorphic/10)*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays61  
xpos 567  
ypos 1613  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-(parent.Anamorphic/10)*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays62  
xpos 567  
ypos 1639  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set Ne5a5c720 [stack 0]  
push $Nfd3744c0  
ZSlice {  
Zchan depth.Z  
center 0.9  
dof 0.2  
name ZSlice13  
xpos 884  
ypos 1535  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle17  
xpos 884  
ypos 1578  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.8  
white_clamp true  
name Grade10  
xpos 884  
ypos 1607  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $Ne5a5c720  
push $Nfd3744c0  
ZSlice {  
Zchan depth.Z  
center 0.9  
dof 0.2  
name ZSlice12  
xpos 887  
ypos 1361  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle16  
xpos 887  
ypos 1404  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.79  
whitepoint 0.8  
white_clamp true  
name Grade9  
xpos 887  
ypos 1433  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $Nfcb7d220  
Dot {  
name Dot30  
xpos 561  
ypos 1092  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*.8*RedMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*.8*RedMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays7  
xpos 527  
ypos 1110  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays1.scale==1 ? 0 : tan_falloff"}}  
name Multiply9  
xpos 527  
ypos 1136  
disable {{"GodRays1.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*.8*RedMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*.8*RedMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays8  
xpos 527  
ypos 1162  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays2.scale==1 ? 0 : tan_falloff"}}  
name Multiply10  
xpos 527  
ypos 1188  
disable {{"GodRays2.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*.8*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays55  
xpos 527  
ypos 1219  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*.8*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays56  
xpos 527  
ypos 1245  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{(parent.Anamorphic/10)*.8*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays57  
xpos 527  
ypos 1286  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-(parent.Anamorphic/10)*.8*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays58  
xpos 527  
ypos 1312  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N49cd4060 [stack 0]  
push $Nfd3744c0  
ZSlice {  
Zchan depth.Z  
center 0.7  
dof 0.2  
name ZSlice11  
xpos 894  
ypos 1200  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle15  
xpos 894  
ypos 1243  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.6  
whitepoint 0.8  
white_clamp true  
name Grade8  
xpos 894  
ypos 1272  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N49cd4060  
push $Nfd3744c0  
ZSlice {  
Zchan depth.Z  
center 0.7  
dof 0.2  
name ZSlice10  
xpos 887  
ypos 1034  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle12  
xpos 887  
ypos 1077  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.59  
whitepoint 0.6  
white_clamp true  
name Grade7  
xpos 887  
ypos 1106  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $Nfcb7d220  
Dot {  
name Dot5  
xpos 520  
ypos 766  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*.6*RedMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*.6*RedMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays5  
xpos 486  
ypos 784  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays1.scale==1 ? 0 : tan_falloff"}}  
name Multiply7  
xpos 486  
ypos 810  
disable {{"GodRays1.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*.6*RedMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*.6*RedMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays6  
xpos 486  
ypos 836  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays2.scale==1 ? 0 : tan_falloff"}}  
name Multiply8  
xpos 486  
ypos 862  
disable {{"GodRays2.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*.6*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays51  
xpos 486  
ypos 904  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*.6*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays52  
xpos 486  
ypos 930  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{(parent.Anamorphic/10)*.6*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays53  
xpos 486  
ypos 971  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-(parent.Anamorphic/10)*.6*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays54  
xpos 486  
ypos 997  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N3ac953d0 [stack 0]  
push $Nfd3744c0  
ZSlice {  
Zchan depth.Z  
dof 0.2  
name ZSlice1  
xpos 890  
ypos 877  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle7  
xpos 890  
ypos 920  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.4  
whitepoint 0.6  
white_clamp true  
name Grade2  
xpos 890  
ypos 949  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N3ac953d0  
push $Nfd3744c0  
ZSlice {  
Zchan depth.Z  
dof 0.2  
name ZSlice2  
xpos 880  
ypos 715  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle9  
xpos 880  
ypos 758  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.39  
whitepoint 0.4  
white_clamp true  
name Grade5  
xpos 880  
ypos 787  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $Nfcb7d220  
Dot {  
name Dot1  
xpos 471  
ypos 457  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*.4*RedMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*.4*RedMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays45  
xpos 437  
ypos 475  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays1.scale==1 ? 0 : tan_falloff"}}  
name Multiply5  
xpos 437  
ypos 501  
disable {{"GodRays1.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*.4*RedMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*.4*RedMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays46  
xpos 437  
ypos 524  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays2.scale==1 ? 0 : tan_falloff"}}  
name Multiply6  
xpos 437  
ypos 550  
disable {{"GodRays2.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*.4*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays47  
xpos 437  
ypos 577  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*.4*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays48  
xpos 437  
ypos 603  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{(parent.Anamorphic/10)*.4*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays49  
xpos 437  
ypos 644  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-(parent.Anamorphic/10)*.4*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays50  
xpos 437  
ypos 670  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N57abaab0 [stack 0]  
push $Nfd3744c0  
ZSlice {  
Zchan depth.Z  
center 0.3  
dof 0.2  
name ZSlice9  
xpos 884  
ypos 550  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle14  
xpos 884  
ypos 593  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.2  
whitepoint 0.4  
white_clamp true  
name Grade3  
xpos 884  
ypos 619  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N57abaab0  
push $Nfd3744c0  
ZSlice {  
Zchan depth.Z  
center 0.3  
dof 0.2  
name ZSlice8  
xpos 875  
ypos 383  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle13  
xpos 875  
ypos 426  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
blackpoint 0.19  
whitepoint 0.2  
white_clamp true  
name Grade6  
xpos 875  
ypos 455  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $Nfcb7d220  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1+(parent.Tangental/1000)*.2*RedMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*.2*RedMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays3  
xpos 432  
ypos 176  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays1.scale==1 ? 0 : tan_falloff"}}  
name Multiply4  
xpos 432  
ypos 202  
disable {{"GodRays1.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*.2*RedMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*.2*RedMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays4  
xpos 432  
ypos 228  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays2.scale==1 ? 0 : tan_falloff"}}  
name Multiply3  
xpos 432  
ypos 254  
disable {{"GodRays2.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{parent.Sagittal*.2*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays41  
xpos 432  
ypos 309  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*.2*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays42  
xpos 432  
ypos 335  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{(parent.Anamorphic/10)*.2*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays43  
xpos 432  
ypos 376  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-(parent.Anamorphic/10)*.2*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays44  
xpos 432  
ypos 402  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N558b5120 [stack 0]  
push $Nfd3744c0  
ZSlice {  
Zchan depth.Z  
center 0.1  
dof 0.2  
name ZSlice7  
xpos 873  
ypos 223  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Shuffle {  
in depth  
green red  
blue red  
alpha red  
name Shuffle11  
xpos 873  
ypos 266  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Grade {  
channels rgba  
whitepoint 0.2  
white_clamp true  
name Grade4  
xpos 873  
ypos 295  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N558b5120  
push $Nfcb7d220  
Keymix {  
inputs 3  
name Keymix1  
xpos 210  
ypos 389  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix2  
xpos 327  
ypos 458  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix3  
xpos 327  
ypos 700  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix4  
xpos 389  
ypos 781  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix5  
xpos 389  
ypos 1028  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix6  
xpos 429  
ypos 1100  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix7  
xpos 429  
ypos 1335  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix8  
xpos 458  
ypos 1427  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix9  
xpos 458  
ypos 1685  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 3  
name Keymix10  
xpos 471  
ypos 1758  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Dot {  
name Dot3  
xpos 505  
ypos 1881  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $N58cf1190  
Dot {  
name Dot34  
xpos 30  
ypos 58  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels rgba  
scale {{1+(parent.Tangental/1000)*RedMult.mix*Outwards.mix} {1+(parent.Tangental/1000)*RedMult.mix*Outwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays1  
xpos -4  
ypos 137  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays1.scale==1 ? 0 : tan_falloff"}}  
name Multiply1  
xpos -4  
ypos 163  
disable {{"GodRays1.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
scale {{1-(parent.Tangental/1000)*RedMult.mix*Inwards.mix} {1-(parent.Tangental/1000)*RedMult.mix*Inwards.mix}}  
center {{input.width/2} {input.height/2}}  
from_color {{"tan_falloff==0 ? 1 :100"}}  
name GodRays2  
xpos -4  
ypos 189  
disable {{"parent.Tangental==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Multiply {  
channels {{{parent.GodRays1.channels}}}  
value 0.04333  
mix {{"GodRays2.scale==1 ? 0 : tan_falloff"}}  
name Multiply2  
xpos -4  
ypos 215  
disable {{"GodRays2.scale==1 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels rgba  
rotate {{parent.Sagittal*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays37  
xpos -4  
ypos 293  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
rotate {{-parent.Sagittal*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays38  
xpos -4  
ypos 319  
disable {{"parent.Sagittal==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels rgba  
skew {{parent.Anamorphic/10*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays39  
xpos -4  
ypos 384  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
GodRays {  
channels {{{parent.GodRays1.channels}}}  
skew {{-parent.Anamorphic/10*RedMult.mix}}  
center {{input.width/2} {input.height/2}}  
name GodRays40  
xpos -4  
ypos 410  
disable {{"parent.Anamorphic==0 ? 1 : 0"}}  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Switch {  
inputs 2  
which {{"\[exists parent.input1] & parent.useExtMask | !(parent.magnitude==1)"}}  
name Switch3  
xpos 0  
ypos 1877  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Dot {  
name Dot23  
xpos 34  
ypos 2140  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
set N30c2a2f0 [stack 0]  
Merge2 {  
inputs 3+1  
operation average  
name Merge1  
xpos 0  
ypos 2292  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Dot {  
name Dot26  
xpos 34  
ypos 2354  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
push $Ne44b1d40  
push $Nfd579d40  
push $N30c2a2f0  
push $N13a30db0  
Copy {  
inputs 2  
from0 rgba.red  
to0 rgba.red  
name CopyRed  
xpos -512  
ypos 2130  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Copy {  
inputs 2  
from0 rgba.green  
to0 rgba.green  
name CopyGreen  
xpos -512  
ypos 2168  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Copy {  
inputs 2  
from0 rgba.blue  
to0 rgba.blue  
name CopyBlue  
xpos -512  
ypos 2206  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Copy {  
inputs 2  
from0 rgba.alpha  
to0 rgba.alpha  
name CopyBlue1  
xpos -512  
ypos 2344  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Switch {  
inputs 2  
which {{viewMask}}  
name Switch5  
xpos -512  
ypos 2546  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Reformat {  
type "to box"  
box_width {{Input.width}}  
box_height {{Input.height}}  
resize none  
pbb true  
name Reformat2  
xpos -512  
ypos 2792  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Output {  
name Output1  
xpos -512  
ypos 2864  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 0  
name BlueMult  
xpos 4018  
ypos 176  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 0  
mix 0.865  
name GreenMult  
xpos 1831  
ypos 186  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 0  
name Magnitude  
xpos 5966  
ypos -1193  
disable true  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 0  
name Outwards  
xpos -98  
ypos -139  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 0  
name Inwards  
xpos -216  
ypos -134  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}  
Keymix {  
inputs 0  
mix 0.725  
name RedMult  
xpos -386  
ypos 137  
addUserKnob {20 DiscoveryPanel +INVISIBLE}  
addUserKnob {26 disco_qi_uri l disco_qi_uri: +INVISIBLE T WHL_Astigmatism.groups.nk@compgen.disco.disco3000.show.whalefall:ilm#1}  
}
```