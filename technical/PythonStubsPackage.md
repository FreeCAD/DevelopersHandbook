---
layout: default
---

# Python stubs

Python is a dynamically typed language, so there is no error when compiling
(Yes, Python can compile.
Compiling/interpreting is an abstract concept, language independent.
See more on [stackoverflow](https://stackoverflow.com/a/6889798)).
Instead, an error is reported at runtime
which may significantly slow down developing process.

To overcome this problem, Python introduced
[Type Hints](https://peps.python.org/pep-0484/).
Using an external tool (type checker like
[Mypy](https://mypy-lang.org/),
[Pyright](https://github.com/RobertCraigie/pyright-python)
) it is possible to detect runtime errors.

Also, stub files allow IDE (integrated development environment)
detect possible names for imported classes or method names.

## Installation

### PyPi

It is possible to install stubs directly from
[PyPI](https://pypi.org/project/freecad-stubs/):

```shell
pip install FreeCAD-stubs
```

### From GitHub repo (especially older versions)

To install stubs for older FreeCAD version
(replace `FreeCAD-0-20` with tag/branch you want),
you can run:

```shell
pip install git+https://github.com/ostr00000/freecad-stubs.git@FreeCAD-0-20
```

Or manually using cloning repo:

1. Clone [repo](https://github.com/ostr00000/freecad-stubs):

    ```shell
    git clone https://github.com/ostr00000/freecad-stubs
    ```

2. Choose desired version:

   ```shell
   git checkout FreeCAD-0-20
   ```

3. And install directly:

   ```shell
   pip install freecad-stubs
   ```

## False positives in stubs

Probably not all types are correctly detected.
[Reporting issues](https://github.com/ostr00000/freecad-stubs/issues) are very welcome.
