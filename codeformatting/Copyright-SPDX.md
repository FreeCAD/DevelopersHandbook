
# Copyright - SPDX

SPDX is used to declare metadata about a file, like what license its published under or who owns the copyright. The annotations are usually placed within inline comments at the start of the file.

*In cases where the first line of a file is already occupied by for example a shebang declaration `#!/usr/bin/env bash`, simply place the SPDX annotations on the next lines after.*

<br/>

## License

To declare what license a file falls under, specify the appropriate [License Id] in the following format:

```
SPDX-License-Identifier: ‹License Id›
```

##### C++

```C++
// SPDX-License-Identifier: ‹License Id›
```

##### Python

```Python
# SPDX-License-Identifier: ‹License Id›
```

<br/>

## Copyright

To declare who own the copyright to a file, declare one annotation for every person / organization.

```
SPDX-FileCopyrightText: ‹Year› ‹Entity›
```

`‹Year›` is the year the code was published in.

`‹Entity›` is either a person or an organization.

##### Person

```
SPDX-FileCopyrightText: 1999 Robert Robertson
```

##### Organization

```
SPDX-FileCopyrightText: 2000 Crazy CAD Technologies
```

<br/>

## Examples

#### 1. Internal C++ Source File

You just wrote a new `.cpp` file from scratch and want to license it under the standard license FreeCAD uses ( `LGPL-2.1-or-later` )

```c++
// SPDX-License-Identifier: LGPL-2.1-or-later
// SPDX-FileCopyrightText: 2026 Mike Mc Mickers
// SPDX-FileNotice: Part of the FreeCAD project.

...
```

<br/>

#### 2. External C++ Source File

You want to include some pre-existing code licensed under a compatible but different license to what FreeCAD uses by default.

```c++
// SPDX-License-Identifier: MIT
// SPDX-FileCopyrightText: 1939 The Wizard of Oz
// SPDX-FileNotice: Part of the FreeCAD project.

...
```

<br/>

#### 3. Mixed Python Licensing

You have some existing code licensed under one license but also want to add some code under a different license.

```Python
# SPDX-License-Identifier: LGPL-2.1-or-later AND MIT
# SPDX-FileCopyrightText: 2010 Technology Inc
# SPDX-FileCopyrightText: 2026 Jane Jay Jensen
# SPDX-FileNotice: Part of the FreeCAD project.

...
```

<br/>

#### 4. Executable Python Script

You have a Python script with a Shebang declaration.

```Python
#!/usr/bin/env python3
# SPDX-License-Identifier: LGPL-2.1-or-later
# SPDX-FileCopyrightText: 2011 Nyan Cat
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


[License Id]: https://spdx.org/licenses/
