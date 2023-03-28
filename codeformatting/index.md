---
title: Code Formatting Guidelines
description:
  Guidelines for code formatting in C++ and Python
layout: default
---

# {{page.title}}

{{page.description}}

FreeCAD is written primarily in C++ and Python (with a few files in other languages such as CMake). To smooth the process of merging new contributions it is desirable to minimize formatting changes to existing code, and to use a uniform code formatting style when writing new code. Eventually we expect to standardize the codebase on specific coding styles using Clang Format for C++ and a Python code-formatter (such as Black), but that is a work-in-progress and _should not_ be applied to existing otherwise-untouched code.

## C++

FreeCAD's codebase currently includes a preliminary [.clang-format](https://github.com/FreeCAD/FreeCAD/blob/master/.clang-format) file. Many IDEs will automatically apply these settings when editing files. To minimize disruption of other developers, only new or significantly-refactored code should have the formatting automatically applied. Code not being worked on should not be automatically formatted at this time.

### C++ Copyright Header

The following is the standard C++ copyright header that should be included in all new C++ files (and backported to files a developer is making significant changes to if they do not already have this format).
```
// SPDX-License-Identifier: LGPL-2.1-or-later

/****************************************************************************
 *                                                                          *
 *   Copyright (c) <YEAR> <YOUR NAME> <YOUR EMAIL>                          *
 *                                                                          *
 *   This file is part of FreeCAD.                                          *
 *                                                                          *
 *   FreeCAD is free software: you can redistribute it and/or modify it     *
 *   under the terms of the GNU Lesser General Public License as            *
 *   published by the Free Software Foundation, either version 2.1 of the   *
 *   License, or (at your option) any later version.                        *
 *                                                                          *
 *   FreeCAD is distributed in the hope that it will be useful, but         *
 *   WITHOUT ANY WARRANTY; without even the implied warranty of             *
 *   MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU       *
 *   Lesser General Public License for more details.                        *
 *                                                                          *
 *   You should have received a copy of the GNU Lesser General Public       *
 *   License along with FreeCAD. If not, see                                *
 *   <https://www.gnu.org/licenses/>.                                       *
 *                                                                          *
 ***************************************************************************/
```

## Python

There is no unified Python coding style agreed upon for the FreeCAD codebase: each module currently has its own style. Developers working on an existing module should format their code in the prevailing style of the module they are working in.

### Python Copyright Header

The following is the standard Python copyright header that should be included in all new Python files (and backported to files a developer is making significant changes to if they do not already have this format).

```
# SPDX-License-Identifier: LGPL-2.1-or-later
# ***************************************************************************
# *                                                                         *
# *   Copyright (c) <YEAR> <YOUR NAME> <YOUR EMAIL>                         *
# *                                                                         *
# *   This file is part of FreeCAD.                                         *
# *                                                                         *
# *   FreeCAD is free software: you can redistribute it and/or modify it    *
# *   under the terms of the GNU Lesser General Public License as           *
# *   published by the Free Software Foundation, either version 2.1 of the  *
# *   License, or (at your option) any later version.                       *
# *                                                                         *
# *   FreeCAD is distributed in the hope that it will be useful, but        *
# *   WITHOUT ANY WARRANTY; without even the implied warranty of            *
# *   MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU      *
# *   Lesser General Public License for more details.                       *
# *                                                                         *
# *   You should have received a copy of the GNU Lesser General Public      *
# *   License along with FreeCAD. If not, see                               *
# *   <https://www.gnu.org/licenses/>.                                      *
# *                                                                         *
# ***************************************************************************
```

### Addon Manager

The Addon Manager uses the Black code formatter: all new code for the Addon Manager should be run through Black (with no additional command-line options) prior to submission. It is recommended that developers working on the Addon Manager set up Black as a pre-commit hook. If also using pylint, in the case of style conflicts Black's formatting takes precedence over that suggested by pylint.

Note that in some cases it is allowable to have line lengths that exceed the Black standard of 88 characters per line in order to support Qt's translate functionality. Qt's translation string extractor does not understand Python string concatenation, so to preserve the easy translatability of longer phrases those phrases must be presented as a single line of text.
