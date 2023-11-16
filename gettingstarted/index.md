---
layout: default
---

# Getting Started

Covers how to set up a build environment for contributing to FreeCAD

Working on FreeCAD is similar to working on may other open-source projects. This guide provides a short overview of the process. Much more information can be found at https://wiki.freecad.org.

## Build Environment

To work on FreeCAD you will need CMake, git, a code editor, a C++ compiler, and a Python interpreter. Many different combinations work:

- On Linux, it's common to use vim, emacs, KDevelop, or CLion as your editor. Compilation with GCC and Clang is supported.
- On Windows we support Visual Studio, Visual Studio Code, and CLion for development, all using the MSVC compiler toolchain.
- On MacOS you will need to install the XCode command line tools, and can use XCode, Visual Studio Code, or CLion as your editor.

Other combinations may work as well, these are just the ones that you will be able to get help with most readily on the [FreeCAD Forum](https://forum.freecad.org).

## Dependencies

See also [Dependencies](./dependencies.md)

FreeCAD depends on many other open source projects to provide the basic foundations of the program. There are many ways of installing these dependencies: for details and the complete list, see the following Wiki pages:

- Linux: [https://wiki.freecad.org/Compile_on_Linux](https://wiki.freecad.org/Compile_on_Linux)
- Windows: [https://wiki.freecad.org/Compile_on_Windows](https://wiki.freecad.org/Compile_on_Windows)
- Mac: [https://wiki.freecad.org/Compile_on_MacOS](https://wiki.freecad.org/Compile_on_MacOS)

## Setting up for Development

1. Fork [https://github.com/FreeCAD/FreeCAD](https://github.com/FreeCAD/FreeCAD) on GitHub
2. Clone your fork: for example, on the command line you can use `git clone https://github.com/YourUsername/FreeCAD FreeCAD-src`
3. Set up `pre-commit` (our automatic code-formatter and checker):


    - Install `pre-commit` (either using your system package manager or pip):
      - Debian/Ubuntu: `apt install pre-commit`
      - Fedora: `dnf install pre-commit` (Fedora)
      - Other (pip in PATH): `pip install pre-commit`
      - Other (pip not in PATH): `python -m pip install pre-commit`
    - On a command line, change into your FreeCAD clone, e.g. `cd FreeCAD-src`
    - Run `pre-commit install` (or `python -m pre-commit install`, depending on your PATH)



4. We **strongly** recommend doing an out-of-source build, that is, build FreeCAD and put all generated files in a separate directory. Otherwise, the build files will be spread all over the source code and it will be much harder to sort out one from the other. A build directory can be created outside the FreeCAD source folder or inside:

    - `mkdir build`
    - `cd build`

5. Run CMake, either in via the CMake GUI or on the command line see the wiki compilation page for your operating system for a detailed list of options.
6. CMake will generate project files that can be read by your IDE of choice. See your IDE's documentation for details. In general:

    - On Linux, compile with a command like `cmake --build /path/to/FreeCAD-src` run from your build directory ( or `cmake --build ..` if your build directory is inside FreeCAD-src).
    - On Windows with Visual Studio, build the "ALL_BUILD target" (you will have to change the path to the final executable the first time you try to run that target).
    - On Mac on the command line use `cmake --build /path/to/FreeCAD-src` from your build directory, or if using CLion be sure to "Build All" the first time you run.

7. If you plan on submitting a PR, create a branch:

    - `git branch fixTheThing`
    - `git checkout fixTheThing` (or both commands in one go: `git checkout -b fixTheThing`)

## Submitting a PR

The basic process is:

1. Write some code (and possibly some unit tests)
2. `git add file1.cpp file2.cpp`
3. `git commit -m "Sketcher: Fixed bug in constraints" -m "Added foo to bar. Fixes #1234."`
    - When running `git commit` our pre-commit hooks will run to check your code. If the scripts had to make changes, you will have to `git add` the changed files and run `git commit` again.
4. `git push` to send your changes to GitHub
5. Visit https://github.com/FreeCAD/FreeCAD -- at the top of the screen you should see a yellow banner suggesting you create a Pull Request. Follow the instructions on the site to get the process started.
