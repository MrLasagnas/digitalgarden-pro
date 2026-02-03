---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/fake-reflection-camera/","tags":["nuke","node","3D","camera","reflection"]}
---



## 🧠 Description
Automatically create a inverted camera along an axis

## 🎯 When to use

- When you want to create some reflections without going to rayrender and you have the camera and geo (or can generate one... maybe projecting it ?)

## ⚠️ Known pitfalls

- Need to work on a version with custom plane to mirror.

## 🧩 Dependencies

## 🧪 Alternatives

## 🧾 Code

```
set cut_paste_input [stack 0]
version 13.0 v10
push $cut_paste_input
NoOp {
 name Fake_Reflection_Camera
 selected true
 xpos 1040
 ypos -1336
 addUserKnob {20 User l "Create Camera"}
 addUserKnob {26 _1 l "" +STARTLINE T "Choose an axis to mirror, select a camera and just press the button !"}
 addUserKnob {26 "" +STARTLINE}
 addUserKnob {4 mirror_axis l "Mirror Axis" M {x y z}}
 mirror_axis z
 addUserKnob {22 createCam l "Create Camera" T "import nuke\n\ntn=nuke.thisNode()\naxis = tn\[\"mirror_axis\"].getValue()\n\n\nshotCam = nuke.selectedNode()\n\n#Create and rename new camera\nlinked_cam= nuke.nodes.Camera3()\nlinked_cam.setName(\"ReflectionCamera\")\n\n#Link sensor size and focal to original camera\nlinked_cam\[\"haperture\"].setExpression(shotCam.name()+ \".haperture\")\nlinked_cam\[\"vaperture\"].setExpression(shotCam.name()+ \".vaperture\")\nlinked_cam\[\"focal\"].setExpression(shotCam.name()+ \".focal\")\n\n#For x axis\nif axis == 0:\n    linked_cam\[\"translate\"].setExpression(shotCam.name() + \".translate.x\", 0)\n    linked_cam\[\"translate\"].setExpression(\"-\"+shotCam.name() + \".translate.y\", 1)\n    linked_cam\[\"translate\"].setExpression(shotCam.name() + \".translate.z\", 2)\n    linked_cam\[\"rotate\"].setExpression(\"-\"+shotCam.name() + \".rotate.x\", 0)\n    linked_cam\[\"rotate\"].setExpression(shotCam.name() + \".rotate.y\", 1)\n    linked_cam\[\"rotate\"].setExpression(\"-\"+shotCam.name() + \".rotate.z - 180\", 2)\n    linked_cam\[\"scaling\"].setValue(-1,0)\n    linked_cam\[\"label\"].setValue(shotCam.name()+ \" inverted on x axis\")\n    \n#For y axis\nif axis == 1:\n    linked_cam\[\"translate\"].setExpression(shotCam.name() + \".translate.x\", 0)\n    linked_cam\[\"translate\"].setExpression(shotCam.name() + \".translate.y\", 1)\n    linked_cam\[\"translate\"].setExpression(\"-\"+shotCam.name() + \".translate.z\", 2)\n    linked_cam\[\"rotate\"].setExpression(\"-\"+shotCam.name() + \".rotate.x-180\", 0)\n    linked_cam\[\"rotate\"].setExpression(\"-\"+shotCam.name() + \".rotate.y\", 1)\n    linked_cam\[\"rotate\"].setExpression(\"-\"+shotCam.name() + \".rotate.z\", 2)\n    linked_cam\[\"scaling\"].setValue(-1,1)\n    linked_cam\[\"label\"].setValue(shotCam.name()+ \" inverted on y axis\")\n\n#For z axis\nif axis == 2:\n    linked_cam\[\"translate\"].setExpression(\"-\"+shotCam.name() + \".translate.x\", 0)\n    linked_cam\[\"translate\"].setExpression(shotCam.name() + \".translate.y\", 1)\n    linked_cam\[\"translate\"].setExpression(shotCam.name() + \".translate.z\", 2)\n    linked_cam\[\"rotate\"].setExpression(shotCam.name() + \".rotate.x\", 0)\n    linked_cam\[\"rotate\"].setExpression(\"-\"+shotCam.name() + \".rotate.y\", 1)\n    linked_cam\[\"rotate\"].setExpression(\"-\"+shotCam.name() + \".rotate.z\", 2)\n    linked_cam\[\"scaling\"].setValue(-1,0)\n    linked_cam\[\"label\"].setValue(shotCam.name()+ \" inverted on z axis\")\n\n\n    \n\n\n\n" +STARTLINE}
 addUserKnob {26 "" +STARTLINE}
 addUserKnob {26 infos l "" +STARTLINE T "by MrLasagna v0.1"}
}


```