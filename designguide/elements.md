# Primary Elements

- [Toolbars](#toolbars)

- [Dock Windows](#dock-windows)

  - [Task Panels](#task-panels)

## Toolbars

Toolbar management in complex technical software such as CAD poses unique challenges as there are often hundreds of potential tools, functions, or features which may require ready access within the GUI. The [Pareto-Principle]((laws-of-ux.md#pareto-principle)) (80/20 rule) should be the basis for how toolbars are populated. The 80/20 rule states that 20% of the input results in 80% of the output. This would imply that 20% of a set of tools get 80% of the use. While this may not be a perfect correlating principle, it should help guide a critical thought process about which tools go onto a toolbar and also how they are organized.

**Toolbar Organization Guidelines:**

- Inverse functions (example: functionally similar additive and subtractive operations) should be combined into single tools wherever practicable. Switching between inverse modes should be done with a boolean control such as a toggle/checkbox within the related TaskPanel rather than resulting in separate tools. It minimizes space on toolbars and simplifies icon development.

- In accordance with the [Pareto Principle](./laws-of-ux.md#pareto-principle), where a group of related functions consist of one which covers a majority of cases, a Menu Button should be formed to group those related functions into a single toolbar slot. The most common or likely to be used function should be presented as the default from that group on the toolbar.

- Toolbars should be broken into logical groupings, both visually(iconography), and functionally. Avoid use of many specialized toolbars in favor of more moderate sized toolbars. ie. one for basic functionality, and one for advanced. (80/20 rule helps here as well. Ideally there would be no more than 3 QToolBars in horizontal orientation or 2 toolbars in a vertical. Those are not hard requirements however.

**Toolbar Buttons:**

- Should consist of only QToolButton or QMenuButton widgets. Other widgets may be used in special circumstances. The developer must ensure that the introduced widget is visually consistent and does not stand out above other tools. Features adding custom widget to toolbars shall be reviewed by the design working group.

- Toolbars which toggle/indicate a functional state can do so via change in icon (color or inclusion of an indicator)

- Deviations in prescribed widget types allocated to a tool bar must only be accepted after consensus is reached between applicable developers and a thorough review by the design team.

## Dock Windows

These are the dockable panels that typically reside docked to the left, right or bottom edges of the FreeCAD UI.

#### Task Panels:

These panels are a quintessential component of FreeCAD's interface. A task panel is the means by which a user interacts with the current task being performed. Ranging from controlling sketch elements, defining parameters for a feature such as a Chamfer or set up parameters for CNC machining. These critical components **must** be uniform in both layout and sizing. A default width, in accordance with the UI [zones and layout](./zones) definitions is established at 360 pixels.

Here are general guidelines to be followed for a task panel:

- "Ok" and "Cancel" buttons *shall* be located at the top of each panel with a centered alignment.\
Alternative button configurations are inconsistent and not acceptable.

- Minimize the width of the panel as much as possible through good use of space, arrangement of widgets, and [naming](./naming).

- Developers should use the appropriate [interactive control](./interactive) widgets to ensure a consistent experience throughout

- Task Panels can potentially include enormous amounts of information and advanced settings/controls. Utilizing FreeCAD's custom [QSInt](https://freecad.github.io/SourceDoc/d9/d11/namespaceQSint.html) widget class, which provides a 'rolling up' of portions of the panel, is encouraged. Follow the [80/20 rule](laws-of-ux.md#pareto-principle), and by default only expose the most commonly needed controls/data to avoid overwhelming users with information/settings when they are not needed. This allows the user to expand more advanced settings, controls or information when the need arises.

[Return to Design Guide Main Page](.)
