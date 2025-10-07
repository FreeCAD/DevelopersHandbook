---
layout: default
---

# Roadmap

The roadmap provides broad objectives for the direction of FreeCAD development.

## Development Roadmap

This document describes several high-level objectives for the FreeCAD project.  Rather than being a laundry list of development tasks, it focuses on a small set of strategic initiatives that, if achieved, will move FreeCAD closer to an ideal future state.

You can read more about the [rationale for having a roadmap](./rationale).

This roadmap outlines broad strategic objectives. Code contributions from developers will always be evaluated on their merits but are more likely to receive timely attention and be successfully merged if they are consistent with the stated objectives of this roadmap.

## The "Next" Release

These objectives described here are very broad. Some of them are very ambitious and will take a long time to achieve.  Others will never be fully achieved or 'done'.  For that reason, it's helpful to pick a specific set of goals to focus on for the next release.  [This page](./next) broadly describes the major goals for the next release.

### Objective: Model stability

Robust, stable models are a necessary precursor to widespread adoption of FreeCAD.  Some types of breakage are the unavoidable result of poor design practices by the user but FreeCAD must commit itself to reducing or eliminating as many causes of breakage as possible. Focus areas include:

1. Toponaming Resolution and Optimization
2. Reduction of sketcher solver errors
3. UI/UX features that promote better modeling practices and avoid brittleness.

    1. Make good practices easier
    2. Remove / replace tools that allow unstable results
4. Improved stability around linked documents

### Objective:  Assembly

Assembly is a core feature of modern CAD systems.  Without an integrated assembly capability, users are forced to use one of several add-on options.  With competing alternatives available, collaboration and interoperability is negatively affected.  Adoption of an assembly workbench should consider, at a minimum, a base set of features including

1. An assembly data format.  A data standard for the representation of assembly information in the FreeCAD document.  The standard should be well thought-out and usable both by the integrated solution as well as add-on options.
2. A license-compatible solver or the option to include a solver at a later date
3. A minimum set of features to satisfy the 80+% of requirements of typical users

### Objective: Flatten the learning curve

CAD is hard.  These are complicated and powerful tools and the user should be expected to invest appropriate time to learn to use them well.  However, UI/UX inconsistency makes the learning process more difficult than it need be.  The FreeCAD community must pursue a program of UI normalization to make learning FreeCAD easier.

The first step in this process is to stop making things worse by accepting UI submissions that are poorly designed or implemented.  Focus areas:

1. Creation of a UI/UX ‘style book’.  This would be a guidebook for developers to follow when creating core and add-on functionality.  The style book would also be useful when evaluating new submissions that affect user experience.  Eventually, the style book standards can be used to ‘score’ existing workbenches and identify opportunities for improvement.
2. A ‘first run’ wizard to familiarize the user with essential customizations and features.
3. Reduce confusing UI elements
    1. Eliminate redundant tools
    2. Suppress infrequently-used workbenches
    3. Consolidate settings, preferences, and other customization tools
    4. Make toolbar placement more predictable and stable
4. Toolbar placement consistency
5. Make tasks fail safe.  Tasks should always be in transactions and ‘undo’ cleanly. Red errors in the console are unacceptable for normal workflow.
6. Normalize the use of terminology both within FreeCAD and, when possible, with external standards.
7. Improve support for materials and implement consistently across all workbenches

### Objective:  Seamless Extensibility

FreeCAD has an addon manager to allow the user to add niche features.  This provides two important functions.  First, it allows the core application to remain leaner and avoid bloat. Second, it allows for competing solutions early on.  Our approach to addons, however, has been somewhat undisciplined.  As a result, the quality of addons is very uneven.  Some are excellent and behave like native parts of the application.  Others feel and behave inconsistently.  Still others are completely broken or unmaintained.  If we want the addon capability to serve both purposes, additional development is needed.  Focus areas include:

1. Improve the Addon Manager workbench to make it easier for users to find good well-maintained addons.  Maybe a ‘Featured’ section if addon workbench has a responsive maintainer, minimum open bugs.   Maybe a user review?  Without some status, we could end up with lots of cruft polluting the the manager.
2. Establish standards for addons to meet to achieve a ‘recommended’ or ‘featured’ status. Featured Addons should
    1. Have an engaged maintainer
    2. Have minimal high priority open issues tickets
    3. Comply with published style book
    4. Support translation
    5. Support unit schema
    6. Install and function cleanly with only the addon manager.
3. Improve Scriptability
    1. Improved editor / support external editor
    2. Native debugger
    3. Improved API

### Objective:  Have the best documentation possible

Documentation has become as important as the FreeCAD application itself. Good documentation can offer a bridge to new users and smooth the learning curve. It can make all the difference between an application that is hard to use and one that is easy to learn.

The FreeCAD documentation is generally in a very good state already. But it could, and should, go much further.  Focus areas include

1. Low-friction to revision through the browser
2. Git-trackable
3. Versions of the documentation that match software versions
4. Good translation support
5. Support for off-line readers
6. Natural integration of Developer / API documentation

### Objective: UI modernization and beautification

We want the FreeCAD application to be both efficient and attractive.  We want it to integrate well with the rest of the desktop experience, to be responsive and to be delightful to use. Focus areas include:

1. User customization
    1. themes
2. Modern UI concepts
    1. Eg “pie menu”
    2. Context menus
    3. Ribbon Bar
3. Efficient use of screen area
    1. Transparency
    2. Multi-screen support
4. Enhanced visualization
    1. Shadows
    2. Simplified ray tracing and rendering

### Objective: Streamlined workflow

Performing common and repetitive tasks should be as effortless as possible.  FreeCAD developers should commit themselves to building efficient workflows throughout the application.  This means minimizing clicks and unnecessary dialogs.  It means adding dedicated tools for routine functions rather than relying on generalized tools that may require more steps. It means anticipating and giving the user efficient access to information when and where they need it.  Focus areas include:

1. Better and consistent measuring tools
2. Improve sketcher :
    1. Missing tools :
        1. Offset tool.
        2. Circular pattern
        3. Usable rectangular array.
        4. Constrain contextually.
3. Seamless copy-paste of geometries/sketches
4. Uniform selection modes
5. Uniform approach to user parameters
6. Simplified access to commonly needed data
    1. Volume
    2. Mass
    3. Moment
    4. Center of Gravity
    5. Surface area
7. Text alignment tool

### Objective: Lower Barriers to Entry

FreeCAD has an enormous number of functions and for a new user, there is no obvious way to use just a task oriented subset.  It should be possible for new users to access a subset of FreeCAD functionality, say on the level of a TinkerCAD.  As they gain experience, they can use additional functions.

FreeCAD has a major terminology problem for new users.  We have a Part workbench that doesn't make Parts and a Part Design workbench that isn't used to design Parts.  We have Pads and Extrudes, Pockets and Cuts, Fuses and Unions, etc.  We have a Part container and a Group container(?) but no core functions to populate a container (ex a core assembly function)..

Forum comment regarding nomenclature: [https://forum.freecad.org/viewtopic.php?p=669869#p669869](https://forum.freecad.org/viewtopic.php?p=669869#p669869)

### Objective: Prevent Shooting Yourself in the Foot

FreeCAD allows users to do things that are known to build fragile models, such as building features based on variable foundations (ex Sketches on Faces, Dimensions based on drawing lines).  We even have icons and commands for doing this.  FreeCAD should prevent "dangerous" actions, convert them behind the scenes to safe actions or at least provide a warning about unsafe practices.
