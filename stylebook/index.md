---
title: Stylebook
description:
   The stylebook provides guidelines and explanations for visual elements in the FreeCAD application
layout: default
---

# {{page.title}}

{{page.description}}

This book is meant to document the general style and conventions of the FreeCAD User Interface.  Guidelines, terminology, and philosophies described here will be used to justify changes in the current UI in order to make the overall user experience more consistent, predictable, intuitive, and attractive in the future.

In the short term, this book will undergo massive change and reorganization.  It will also serve as an important part of a larger discussion.  Feel free to link to existing issues and pull requests, and projects.

In the long term, the book should settle down and become primarily a resource for developers and designers to refer to.  Most links should be removed.


## Parts of the UI

![Parts of the UI]()

1. Workbench switcher
2. Toolbars
3. Toolbar area
4. Comboview
5. Tree
6. Property Pane
7. The MDI view
8. The MDI Selector
9. The Report view
10. The python console
11. Status Bare
12. Nav Cube

### Workbench Switcher

The workbench switcher is an important control to general FreeCAD workflow.  It is the primary way for the user to switch workbench contexts.

#### Guidelines

1. The workbench switcher should always be available to the user.  It shouldn’t jump around.  

#### Problems

1. The switcher itself has no accelerator key associated with it.  The user must click with a mouse.
2. The first ten workbenches are automatically assigned an accelerator of W &lt;#>.  The user has no control over the assignment of accelerators
3. By default, all workbenches installed in FreeCAD are shown.  This includes workbenches that are of marginal value or seldom used, especially by new users. This contributes to confusion since users don’t know yet which workbenches they need to use.

### Toolbars

1. File
2. Edit
3. Clipboard
4. Workbench
5. Macro
6. View
7. Structure
8. Help
9. Navigation
10. <&lt;Workbench>>

#### Guidelines

1. Toolbars provide convenient access to frequently used commands. They do this at the expense of permanently occupying screen real space.
2. Too many toolbars creates visual clutter.  All tools compete for the users attention simultaneously.  They are not context aware.
3. Toolbars group related commands
4. Anticipate the user disable the toolbar.
5. Group locally similar tools into a toolgroup
6. All tools in a toolbar should have a translatable and useful tooltip
7. The toolbar should have a translatable and userful tooltip
8. A toolbar should contain a minimum number of tools, ideally less than seven. Longer toolbars provide fewer options for customization and often get shortened or clipped offscreen.  They also increase cognitive burden since the user has to process the group as a whole.

#### Problems

1. Toolbars are the developers go-to UI element.  They are simple to code and useful during development.  Asa result they are often overused and visually cluttered.

### Toolbar Area

The toolbar area contains toolbars.  Some of the toolbars are available everywhere and some are workbench specific. 

#### Guidelines

1. Consider how toolbars will look and arrange on small screens
2. Consider how toolbars will look and arrange on multi-screen configurations

#### Problems

1. Toolbars can be rearranged on a per-workbench basis and the arrangement is meant to persist between sessions.  This is intended to be a feature but often acts like a bug.   The user arranges workbenchs (including those that appear on multiple workbenches) while in one bench.  Then switches to anothe bench and the toolbars move to different locations.  

### ComboView

![A vision for the comboview](./comboview.png)

#### Guidelines

1. Item

#### Problems

1. Item

### Tree

#### Guidelines

1. Item

#### Problems

1. Item

### Property Pane

The property pane shows properties of the selected object.  Properties are in groups.  Right clicking give an ‘autoexpand’ option and a ‘show all’ option.  The developer has discretion about which properties are automatically shown.  In practice almost all options are available because the developer probably hasn’t thought about it.  Many of these could probably be hidden by default to make the list shorter and less cluttered.

#### Guidelines

1. Item

#### Problems

1. Item

### MDI View

The MDI view has the most usable space.  This is almost always the 3D model except for spreadsheet and TechDraw.  It could be used for other tasks when the user’s focus is needed for a long time or to give a maximum view of something (raytraced image)

#### Guidelines

1. Item

#### Problems

1. Item

### MDI Selector

#### Guidelines

1. Item

#### Problems

1. Item

### Report View

#### Guidelines

1. Item

#### Problems

1. Item

### Python Console

#### Guidelines

1. Item

#### Problems

1. Item

### Status Bar

The status bar is the only part of the UI that is always the same regardless of workbench, task or mode.  That makes it special.  

#### Guidelines

1. The real estate is very limited so it should be protected and used very sparingly and wisely.  For example the mouse model switcher probably doesn’t need a label.  An icon and tooltip is sufficient and would use only 1/4 of the space it currently occupies.

#### Problems

1. Item


### Nav Cube

General

#### Guidelines

1. Item

#### Problems

1. Item

The combo view contains the tree and the task panel area

Goal for combo view : 

![alt_text](images/image1.png "image_tooltip")


### Task view

Each tool should put all of it’s settings in a single task box.

Task panels should never require scrolling.

Double clicking on an item in the tree will show a task panel if one is available.  If there was a task panel used to create an object, double clicking should show THE SAME task panel to edit the object. (I’m looking at YOU, Draft workbench!  You too, Arch)

Task panels should always produce a valid output if ‘OK’ button is pressed.  If a valid output isn’t possible, don’t enable the ‘ok’ button. ->This is interesting but I wonder how it could be done.

Task panels should always start with valid defaults (e.g. the shapestring dialog should have a default font.  It should preview the text in the 3D window.

Task panels should always preview their results in the 3d window.  If preview is too computationally intense, provide an ‘apply’ button to generate a preview.  (I’m looking at YOU, shapestring tool!)  


### Selection

[Reference: https://wiki.freecad.org/Selection_methods](https://wiki.freecad.org/Selection_methods)


![The Selectionview](./selection.png)

#### Terms

1. Selection: the set of objects and subojects that the  user has selected.
2. Pre-selection: the object directly under the pointer which will be added to the selection if the user clicks
3. Selection View.  The panel which allows the user to see the selection items
4. Greedy selection.  Mode in which additional items are added to the selection without requiring a modifier key
5. Selection gate  Logic which can be initiated via code that controls which objects may be selected.  
6. Selection observer  Logic which can be initiated via code that watches the selection. It can be used to trigger action under specific selection conditions.

#### Guidelines

1. Selection gates and observers are powerful tools for adjusting the affordances of the GUI and providing signals.  They must be used with care because they can adversely affect the performance of the GUI. 
2. If the user is asked to select multiple features, he should be able to select by clicking them, and unselect them as well by clicking on selected features.


## Context Menus

some text

### Guidelines


## Navigation

### Elements

1. Nav Cube
2. Standard Views
3. Keyboard accelerators

#### Nav cube

#### Standard views
## Widgets

#### Guidelines

1. FreeCAD has a custom QT widget for displaying units.  The control will convert the displayed units based on the users selected unit schema.


## Tables

When a table is used to display rows of items with multiple elements, such as the workbench preference page, the first column should be the icon/name of the element. This first column can be preceded by a checkbox only if it is an activation checkbox. The rest of UI elements should be put after, in an order that makes the most sense.

## Terminology

FreeCAD has a major terminology problem for new users.  We have a Part workbench that doesn't make Parts and a Part Design workbench that isn't used to design Parts.  We have Pads and Extrudes, Pockets and Cuts, Fuses and Unions, etc.  We have a Part container and a Group container(?) but no core functions to populate a container (ex a core assembly function).

## Philosophy
There should be one-- and preferably only one --obvious way to do it.  Developers are notorious for giving users options thinking that is a good thing.  In reality, every tool takes time to understand.  Given two nearly identical implementatoins of hte same thing, the user has to figure out how they differ.  Don’t make the user think.


## Resources
Decent book. It’s mostly about web usability but the core ideas are sound.
[https://www.amazon.com/Dont-Make-Me-Think-Usability/dp/0321344758](https://www.amazon.com/Dont-Make-Me-Think-Usability/dp/0321344758)

This looks interesting
[https://www.amazon.com/Laws-UX-Psychology-Products-Services-ebook/dp/B087F24RJB/ref=sr_1_12?keywords=user+interface+design&qid=1679441490&s=books&sprefix=user+interface%2Cstripbooks%2C121&sr=1-12](https://www.amazon.com/Laws-UX-Psychology-Products-Services-ebook/dp/B087F24RJB/ref=sr_1_12?keywords=user+interface+design&qid=1679441490&s=books&sprefix=user+interface%2Cstripbooks%2C121&sr=1-12)

Classic - great introduction to design concepts like affordances and signals
[https://www.amazon.com/Design-Everyday-Things-Revised-Expanded-ebook/dp/B00E257T6C/ref=sr_1_17?keywords=user+interface+design&qid=1679441706&s=books&sprefix=user+interface%2Cstripbooks%2C121&sr=1-17](https://www.amazon.com/Design-Everyday-Things-Revised-Expanded-ebook/dp/B00E257T6C/ref=sr_1_17?keywords=user+interface+design&qid=1679441706&s=books&sprefix=user+interface%2Cstripbooks%2C121&sr=1-17)
