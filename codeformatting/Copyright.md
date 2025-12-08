
# Copyright

When creating new files in the FreeCAD project, you should - if possible - declare an `SPDX License Identifier` with the appropriate license.

In source files it is currently also required to add a `Copyright Header`.

[» Copyright Snippets](./Copyright-Snippets)

<br/>

## SPDX License Identifiers

SPDX is a simple way of declaring what license something falls under and is declared by adding a comment in the following format to a file:

```
SPDX-License-Identifier: <License>
```

1.  The comment should be placed at the start of a file.

    *An example for an exception to this rule are shebang declarations ( `#!` ), which should come first.*

2.  You have to use the appropriate [License Identifier].

<br/>

### Examples

#### 1. Internal C++ Source File

You just wrote a new `.cpp` file from scratch and want to license it under the standard license FreeCAD uses ( `LGPL-2.1-or-later` )

```c++
// SPDX-License-Identifier: LGPL-2.1-or-later
// SPDX-FileNotice: Part of the FreeCAD project.

...
```

<br/>

#### 2. External C++ Source File

You want to include some pre-existing code licensed under a compatible but different license to what FreeCAD uses by default.

```c++
// SPDX-License-Identifier: MIT
// SPDX-FileNotice: Part of the FreeCAD project.

...
```

<br/>

#### 3. Mixed Python Licensing

You have some existing code licensed under one license but also want to add some code under a different license.

```Python
# SPDX-License-Identifier: LGPL-2.1-or-later AND MIT
# SPDX-FileNotice: Part of the FreeCAD project.

...
```

<br/>

#### 4. Executable Python Script

You have a Python script with a Shebang declaration.

```Python
#!/usr/bin/env python3
# SPDX-License-Identifier: LGPL-2.1-or-later
# SPDX-FileNotice: Part of the FreeCAD project.

...
```

<br/>

#### 5. Icons

Icons should have whatever license the author intended, for example `CC-BY-SA-4.0`.

In `svg` files this is declared via metadata, not as a SPDX comment.

<br/>

#### 6. Documentation

Besides the wiki, currently we don't license documentation, however you might want to consider putting it under the `Unlicense` or `CC-BY-SA-4.0`.

<br/>

## Copyright Headers

Copyright headers are currently required to be added to files if **ALL** of the following conditions apply:

1.  The file is `C++` or `Python` source code

2.  The file is solely licensed under `LGPL-2.1-or-later`

3.  The code has been written for FreeCAD

    ( Do not mark included libraries with this )

<br/>

### Declarations

To declare the copyright holders, simply follow this format:

`© <Year> <Entity>`

- `<Year>` is the current year / the year the code was published.

- `<Entity>` is either you - the person - or your organization.

#### Examples

##### Person

`© 1999 Robert Robertson`

##### Organization

`© 2000 Crazy CAD Technologies`


<br/>

### Languages

### C++

```C++
/******************************************************************************
 *                                                                            *
 *   © <Year> <Entity>                                                        *
 *                                                                            *
 *   FreeCAD is free software: you can redistribute it and/or modify          *
 *   it under the terms of the GNU Lesser General Public License as           *
 *   published by the Free Software Foundation, either version 2.1            *
 *   of the License, or (at your option) any later version.                   *
 *                                                                            *
 *   FreeCAD is distributed in the hope that it will be useful,               *
 *   but WITHOUT ANY WARRANTY; without even the implied warranty              *
 *   of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.                  *
 *   See the GNU Lesser General Public License for more details.              *
 *                                                                            *
 *   You should have received a copy of the GNU Lesser General Public         *
 *   License along with FreeCAD. If not, see https://www.gnu.org/licenses     *
 *                                                                            *
 ******************************************************************************/
```

<br/>

### Python

```Python
################################################################################
#                                                                              #
#   © <Year> <Entity>                                                          #
#                                                                              #
#   FreeCAD is free software: you can redistribute it and/or modify            #
#   it under the terms of the GNU Lesser General Public License as             #
#   published by the Free Software Foundation, either version 2.1              #
#   of the License, or (at your option) any later version.                     #
#                                                                              #
#   FreeCAD is distributed in the hope that it will be useful,                 #
#   but WITHOUT ANY WARRANTY; without even the implied warranty                #
#   of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.                    #
#   See the GNU Lesser General Public License for more details.                #
#                                                                              #
#   You should have received a copy of the GNU Lesser General Public           #
#   License along with FreeCAD. If not, see https://www.gnu.org/licenses       #
#                                                                              #
################################################################################
```

<br/>

[License Identifier]: https://spdx.org/licenses/
