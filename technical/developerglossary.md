---
layout: default
---

# Developer's Glossary

Some terms that a developer may run across.

Note that there may be subtle differences in how some terms are used by developers vs how they are used by users.

* Binding: the mechanism for linking C++ functionality to and from Python.
* CI (Continuous Integration): the mechanism by which Pull Requests are automatically built and unit tested.
* Feature: generally, an object derived from App::DocumentObject.  Also used to refer to a single step in the evolution of a PartDesign Body.
* LibPack: a library of FreeCAD dependencies used on the Windows platform.
* Preference: an entry in user.cfg.  Also called a parameter.
* Quarter: a widget which uses the Qt QGraphicsView to display Coin3d scenegraphs.
* Pull Request: how a contributor indicates that they have a change to be merged
* ViewProvider: an object that provides painting services for a Feature.  The ViewProvider is notified of changes in a Features properties and, if the change affects the visual representation of the Feature, updates the display.  Note that in some modules (ex TechDraw with the Qt GraphicsFramework) there is another layer of functionality that manages the painting.

## See Also

* [FreeCAD glossary wiki entry](https://wiki.freecad.org/Glossary)
* [Contributing](https://github.com/FreeCAD/FreeCAD/blob/master/CONTRIBUTING.md)
