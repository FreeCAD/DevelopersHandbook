---
layout: default
---

# Code Formatting Guidelines

Guidelines for code formatting in C++ and Python

FreeCAD is written primarily in C++ and Python (with a few files in other languages such as CMake). To smooth the process of merging new contributions it is desirable to minimize formatting changes to existing code, and to use a uniform code formatting style when writing new code. Eventually we expect to standardize the codebase on specific coding styles using Clang Format for C++ and a Python code-formatter (such as Black), but that is a work-in-progress and _should not_ be applied to existing otherwise-untouched code.

## Copyright

When creating new files in the FreeCAD project, you should - if the file format allows - add the appropriate SPDX annotation.

In source files like Python & C++ it is also required to add a copyright header.

[» How to add SPDX annotations](./Copyright-SPDX)

[» How to add Copyright Headers](./Copyright-Headers)

[» Snippets for your convenience](./Copyright-Snippets)

## Setting up pre-commit

For the sections of the code that have already been auto-formatted, we use [git pre-commit hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) to enforce that formatting. We use the [pre-commit](https://pre-commit.com/) framework to install and manage our hooks. All developers should install pre-commit following the [instructions here](https://freecad.github.io/DevelopersHandbook/gettingstarted/). When making a new commit, the hooks examine your changed files and ensure that they follow the appropriate formatting for the section of code you are working on. You do not need to manually yourself with your code's formatting as you develop, and may code it as you like (or as your IDE likes...). The hook will ensure that your finished code conforms to project requirements.

Not all of FreeCAD currently uses these hooks: it is up to the Maintainers in charge of a particular Workbench or subdirectory to work with their development team to migrate their code's formatting to the automated tools, and to then enable the hooks on their subdirectory.

Currently the following sections of code are covered by pre-commit: `src/Mod/AddonManager`, `src/Tools`, and `tests/src`.

## C++

FreeCAD's codebase currently includes a [.clang-format](https://github.com/FreeCAD/FreeCAD/blob/master/.clang-format) file. Many IDEs will automatically apply these settings when editing files. To minimize disruption of other developers, only new or significantly-refactored code should have the formatting automatically applied. Code not being worked on should not be automatically formatted at this time.

For naming conventions, please see [Naming Things]({% link bestpractices/codereview.md %}#naming-things).

## Python

There is no unified Python coding style agreed upon for the FreeCAD codebase: each module currently has its own style. Developers working on an existing module should format their code in the prevailing style of the module they are working in.

## Inline documentation

You will find a mix of different generations of comment-based inline documentation systems in the code base depending on when the code was written.  As of 2024 it is preferred that you write [doxygen](https://www.doxygen.nl/manual/docblocks.html) compatible comment documentation.  For methods, including the @param and @return as appropriate is highly recommended.
