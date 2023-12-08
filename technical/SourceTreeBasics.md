---
layout: default
mermaid: true
---

# Partial View of the FreeCAD Source Tree

A picture of the most commonly encountered branches of the tree.

The full FreeCAD source tree has many other branches, but most Contributors will
only need to deal with these:

![The FreeCAD Source Tree](./ressources/SourceTreeBasics.svg)

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
