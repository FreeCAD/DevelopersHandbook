---
layout: default
---


## Subpages
[Glossary](./glossary.md)  [Navigation](./navigation.md)  [Selection](./selection.md)  [Toolbars](toolbars.md)



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
2. Too many toolbars create visual clutter. All tools compete for the users attention simultaneously. They are not context aware.
3. Toolbars group related commands.
4. Anticipate the user might disable the toolbar.
5. Group locally similar tools into a toolgroup.
6. All tools in a toolbar should have a translatable and useful tooltip.
7. The toolbar itself should have a translatable and useful tooltip.
8. A toolbar should contain a minimum number of tools, ideally less than seven. Longer toolbars provide fewer options for customization and often get shortened or clipped offscreen. They also increase cognitive burden since the user has to process the group as a whole.

#### Problems

1. Toolbars are the developers go-to UI element. They are simple to code and useful during development. As a result they are often overused and visually cluttered.

### Toolbar Area

The toolbar area contains toolbars. Some of the toolbars are available everywhere and some are workbench specific. 

#### Guidelines

1. Consider how toolbars will look and arrange on small screens
2. Consider how toolbars will look and arrange on multi-screen configurations

#### Problems

1. Toolbars can be rearranged on a per-workbench basis and the arrangement is meant to persist between sessions. This is intended to be a feature but often acts like a bug.   The user arranges toolbars to his liking (including those toolbars that appear on multiple workbenches) while in one workbench. After a switch to another workbench it can happen that the toolbars rearrange in a different order.  

