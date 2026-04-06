# Python Practices

Work in Progress - looking for help!

# Naming conventions - PEP8

Using well known conventions makes easier for other developers to understand your code and maintain it in the future.

* https://peps.python.org/pep-0008/

# Namespacing

Use proper namespacing to avoid conflicts with existing packages. It is preferred to put any FreeCAD code in a sub-module of the `freecad` base namespace.

- https://docs.python.org/3/tutorial/modules.html#packages
- https://packaging.python.org/en/latest/guides/packaging-namespace-packages/

# Tooling

## Code formatting and linting

- **Ruff**: (modern and fast)

  https://docs.astral.sh/ruff/

- **Black**

  https://black.readthedocs.io/en/stable/

  Main FreeCAD repo uses `black` with `--linelength=100` in the pre-commit hook, so if you are submitting code to main, take that into account.



## Project management

The modern tooling for python is `uv`, it replaces `pip` and manage dependencies, project metadata, virtual environments, etc... and it is 10x faster than pip.

https://docs.astral.sh/uv/

# Typing

Type hints makes the code safer and more easy to maintain and refactor.

- cheat-sheet: https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html

There are many static  type checkers:

- mypy (maybe the most common): https://mypy-lang.org/
- pyright (Microsoft): https://github.com/microsoft/pyright
- ty (New and fast): https://github.com/astral-sh/ty

# Python versions

Current supported python versions in FreeCAD are 3.10+, but some addons may not work with latest python versions.
