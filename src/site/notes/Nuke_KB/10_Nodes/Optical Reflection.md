---
{"dg-publish":true,"permalink":"/nuke-kb/10-nodes/optical-reflection/","tags":["nuke","node","relight","rendering","3D"]}
---

Compatible Nuke versions: 14.0 or later
https://www.nukepedia.com/tools/gizmos/3d/optical-reflection/?tab=download

## 🧠 Description

Creates physically based reflections using a world normal pass, a camera and an HDRI as inputs. includes variable roughness, diffuse contribution, thin film interference and more.
## 🎯 When to use


## ⚠️ Known pitfalls

## 🧩 Dependencies
![[OpticalReflection.gizmo]]
## 🧪 Alternatives

## 🧾 Code (menu.py)

```
toolbar = nuke.toolbar("Nodes")
userMenu = toolbar.addMenu("User", icon="Modify.png")

threeDMenu = userMenu.addMenu('3D', 'Cube.png')
threeDMenu.addCommand("OpticalReflection", "nuke.createNode('OpticalReflection')")
```