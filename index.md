# <img src="images/FreeCAD-symbol.svg" width="5%" style="margin-bottom:-2%" /> FreeCAD Developers Guide

Welcome to the FreeCAD Developer's Guide! This is a work-in-progress, so please feel free to submit Issues and Pull Requests when you find areas that need work.

### [Roadmap](./roadmap)
Describes the broad objectives for FreeCAD Development

### [Getting Started](./gettingstarted)
How to set up a development environment to work on FreeCAD.

### [Design Guide](./designguide)
Covers guidelines for User Experience, Interaction and Interface in FreeCAD.

### [Code Formatting Guide](./codeformatting)
Covers the C++ and Python code formatting guidelines.

### [Good Practices / Code Review Guide](./bestpractices)
Covers the C++ and Python code best practices and tips for code reviewers.

### [Maintainers Guide](./maintainersguide)
Gives guidelines to maintainers about code reviews and merge procedures.

### [Technical Guide](./technical)
A guide for developers learning their way around the FreeCAD codebase.

---

## Related Developer Resources

Over time, developer-related content has grown and spread across several dedicated repositories. The following resources complement this handbook and should be consulted for specialized topics:

### [Addon Academy](https://github.com/FreeCAD/Addon-Academy)
Documentation, examples, and guides on how to develop FreeCAD addons. Covers the full addon lifecycle including creation, development, polishing, publishing, and maintenance. If you are interested in building addons or workbenches for FreeCAD, this is the primary resource.

**Key topics covered:**
- [Creating an addon](https://freecad.github.io/Addon-Academy/Guides/Creating/) — project scaffolding with Cookiecutter
- [Developing an addon](https://freecad.github.io/Addon-Academy/Guides/Developing/) — coding, testing, and debugging
- [Publishing an addon](https://freecad.github.io/Addon-Academy/Guides/Publishing/) — packaging and distribution via the Addon Manager
- [Maintaining an addon](https://freecad.github.io/Addon-Academy/Guides/Maintaining/) — versioning, updates, and long-term upkeep
- [Topic deep-dives](https://freecad.github.io/Addon-Academy/Topics/) — licensing, structuring, dependencies, types, and glossary

### [Source Documentation (SourceDoc)](https://github.com/FreeCAD/SourceDoc)
Doxygen-generated API documentation for the FreeCAD C++ source code. This provides class hierarchies, method signatures, and detailed reference documentation for core modules.

> **Note:** The SourceDoc is currently based on FreeCAD version 0.20 and may not reflect the latest API changes in the development branch. For the most up-to-date API information, consider generating the documentation locally from the source tree, or consult the [Python stubs package](./technical/PythonStubsPackage) for Python-level API reference.

### [Ondsel Lens Documentation](https://github.com/FreeCAD/lens-docs)
Documentation for [Ondsel Lens](https://lens.ondsel.com/), a free/libre product data management (PDM) system designed for FreeCAD and FreeCAD-based software. Covers the web interface for managing CAD data, version control, and collaboration features.

This is relevant for developers building workflows that integrate with cloud-based PDM or collaborative design features.
