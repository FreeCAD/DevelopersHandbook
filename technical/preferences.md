---
layout: default
---

#  Handling Preferences

How to retrieve and update user preferences.

The `user.cfg` XML file contains a hierarchical list of all the preferences used by FreeCAD.

Each module has its own branch of the hierarchy starting under Preferences/Mod.

There are two ways of updating preferences:
* the Tools > Edit parameters menu entry
    * this is a general purpose preference editor that can change any parameter in the hierarchy. This editor does not perform any validation or suggest valid values.
* custom preference dialogs
    * [a sample dialog](https://github.com/FreeCAD/FreeCAD/blob/master/src/Mod/Part/Gui/DlgSettingsMeasure.cpp)
    * these dialogs use [custom FreeCAD widgets](https://wiki.freecad.org/Compile_on_Windows/en#Qt_Designer_plugin) to update preference values.  Each preference widget has two special properties: prefEntry and prefPath.  prefPath is the identifies the branch of the preference hierarchy and prefEntry identifies the leaf node to be updated.
    * two entries are needed in the dialog for each preference value - onSave() in saveSettings and onRestore() in loadSettings().

Preference values are retrieved (and sometimes set) using functions from [Base/Parameter.cpp](https://github.com/FreeCAD/FreeCAD/blob/master/src/Base/Parameter.cpp).

Good examples of retrieving parameters can be found in:
* Python: [get_param()](https://github.com/FreeCAD/FreeCAD/blob/master/src/Mod/Draft/draftutils/utils.py) in /Mod/Draft/draftutils/util.py
* C++: [various methods](https://github.com/FreeCAD/FreeCAD/blob/master/src/Mod/TechDraw/App/Preferences.cpp) in /Mod/TechDraw/App/Preferences.cpp

## See also

* [Preference Editor wiki entry](https://wiki.freecad.org/Preferences_Editor)
