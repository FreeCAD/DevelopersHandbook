---
title: "Partial View of the FreeCAD Source Tree"
description:
    "A picture of the most commonly encountered branches of the tree."
layout: default
mermaid: true
-—
# Partial View of the FreeCAD Source Tree

The full FreeCAD source tree has many other branches, but most Contributors will
only need to deal with these:


```mermaid
graph LR
    A[src] --- B[App: Documents, DocumentObjects, EventPropagation]
    A --- C[Base: FundamentalTypes, Utilities]
    A --- D[Gui: Windows, Menus, Dialogs]
    A --- E[Mod: ApplicationLogic]
    subgraph Mod: ApplicationLogic
    E[Mod] --- F[Arch: Buildings]
    E --- G[Draft: 2DDesign]
    E --- H[Part: BasicShapes, BooleanOps]
    E --- I["..."]
    end
```

