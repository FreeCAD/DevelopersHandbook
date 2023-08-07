---
layout: default
---

# Maintaining the FreeCAD LibPack for Windows

To create a new LibPack for Windows, begin by creating an empty directory. The suggested naming scheme
is to name it "LibPack-XX-YY-ARCH" where XX is the release of FreeCAD the pack is intended to work with,
YY is a version number of the LibPack itself, and ARCH is the architecture the LibPack was build for
(e.g. x86_64 or arm64). The rest of this document will refer to that directory as *LIBPACK*.

Note that in several places below you are instructed to load a developer's power shell: you may use
whichever shell you'd like, but ensure that you are loading the shell configured with the x64 native
compilation tools if you are compiling on an Intel processor. ARM compilation is untested.

In most of the instructions below, code is cloned directly from a first-party git repository. In most
cases it is perfectly feasible to directly download a source release package instead of cloning the
entire git repository.

For the most part, the libraries below are self-compiled in order to reduce dependency on third-party
packagers. In some cases it is feasible to download precompiled binaries of the individual items. Major
exceptions here are that both Python and Qt are used in their precompiled forms since the packages are
provided for Windows by the first parties in each case. libclang is provided by the Qt project as part
of the dependencies of PySide6, but can be self-compiled if desired.

WORK IN PROGRESS!!

Install the following packages into the subdirectory you created for this LibPack. In general the order below should work, with
dependencies being installed before the software which requires them. Note that not all packages use the same installation
structure, so the final LibPack contains several different types of installation tree.
* **[Python](https://python.org)** -- Install in a newly-created *LIBPACK\bin* subdirectory using the basic Python installer. Only install
  Python itself: the Idle IDE, debug symbols, etc. are not included in the LibPack.
* **[Qt](https://qt.io)** -- The Open Source installer is found at https://www.qt.io/download-open-source
* Most of the optional components of Qt can be omitted, except for:
  - Qt WebEngine
  - Qt PDF
  - Qt Image Formats
* Qt should be installed into an arbitrary location outside of the LibPack you are creating: the Qt installer
  will create a subdirectory structure that is based on the version number and architecture of the installation.
  Within that subdirectory are many directories: `bin`,`doc`,`include`, etc. These should all be copied into
  the top level of the LibPack
* **[Boost](https://boost.org)** -- this is split into its headers and libraries:
  - Many Boost libraries are header-only, but FreeCAD does require several compiled libraries
  - Follow the Boost instructions for compiling: the easiest is to run `bootstrap` from a developer's command prompt
  - Copy the libraries from *BOOST_ROOT\stage\lib* into *LIBPACK\lib*
  - Copy the entire *BOOST_ROOT\boost* directory into *LIBPACK\include*
* **[Coin](https://www.coin3d.org)** -- obtained by cloning and compiling https://github.com/coin3d/coin
  - Make sure you compile against the Boost libraries downloaded above by setting BOOST_ROOT to the top of the *LIBPACK*
  - Set CMAKE_INSTALL_PATH to the top of the *LIBPACK*
  - Add include directory to find the Boost include files by editing CXXFLAGS and adding "/I C:\path\to\libpack\include" to it
  - Do not build the Tests or Documentation
  - In Visual Studio, run the ALL_BUILD and then INSTALL targets in Release mode.
* **[Quarter](https://www.coin3d.org)** -- obtained by cloning and compiling https://github.com/coin3d/quarter
  - Set Coin_DIR to *LIBPACK/lib/cmake/Coin-4.0.1* (or the current version, if it is not 4.0.1)
  - Set Qt6_DIR to *LIBPACK/lib/cmake/Qt6*
  - Set CMAKE_INSTALL_PREFIX to *LIBPACK*
  - In some cases even with Qt6_DIR specified, Quarter will fail to find all necessary Qt library cmake files. In
    those cases you will need to manually set the path to the cmake files as *LIBPACK/lib/cmake/Qt6XXX*.
  - Do not build the examples, tests, or documentation
  - At this time this guide was written, Quarter did not correctly support Qt 6.5: the file QuarterWidgetP.cpp had to
    be modified to add `#include <QOpenGLContext>` in the Qt includes section at the top of the file.
  - Build the ALL_BUILD and INSTALL targets in Release mode
* **[SWIG](https://www.swig.org)** -- obtained from https://www.swig.org, prebuilt binaries are available for Windows
  - Copy the downloaded folder to *LIBPACK\bin\swig* (e.g. change its name to remove the version number)
* **[Pivy](https://www.coin3d.org)** -- obtained by cloning and compiling https://github.com/coin3d/pivy
  - Set Python_ROOT_DIR to *LIBPACK/bin*
  - Set SWIG_EXECUTABLE to *LIBPACK/bin/swig/swig.exe*
  - Set SWIG_DIR to *LIBPACK/bin/swig/Lib* (note the "Lib" at the end of the path!)
  - Set Coin_DIR to *LIBPACK/lib/cmake/Coin-4.0.1* (or the current version, if it is not 4.0.1)
  - NOTE: Do *not* install SoQt -- doing so will trigger a dependency on Qt5
  - Set Qt6_DIR to *LIBPACK/lib/cmake/Qt6*
  - In some cases even with Qt6_DIR specified, Quarter will fail to find all necessary Qt library cmake files. In
    those cases you will need to manually set the path to the cmake files as *LIBPACK/lib/cmake/Qt6XXX*.
  - Set CMAKE_INSTALL_PREFIX to *LIBPACK*
  - Build the ALL_BUILD and INSTALL targets in Release mode
* **[libclang](https://clang.llvm.org)** -- Needed by pyside6, and provided prebuilt by Qt
  - Download from https://download.qt.io/development_releases/prebuilt/libclang/
  - Decompress and move the contents to the *LIBPACK* directory
* **[Pyside](https://pypi.org/project/PySide6)** -- now part of the Qt project, installed as a Python package with pip
  - E.g. `LIBPACK/bin/python.exe -m pip install pyside6==6.5.2`
  - Note that this will also install Shiboken, in the same location
* **[VTK](https://vtk.org)** -- Download or clone the source
  - Set CMAKE_INSTALL_PREFIX to *LIBPACK*
  - Build the ALL_BUILD and INSTALL targets in Release mode
* **[HarfBuzz](https://harfbuzz.github.io)**
  - Clone https://github.com/harfbuzz/harfbuzz
  - Switch to the latest release branch
  - Set PYTHON_EXECUTABLE to *LIBPACK/bin/python.exe*
  - Set CMAKE_INSTALL_PREFIX to *LIBPACK*
  - Build the ALL_BUILD and INSTALL targets in Release mode
* **[ZLIB](http://zlib.net)**
  - Download and unpack the latest source code
  - Set CMAKE_INSTALL_PREFIX to *LIBPACK* (note: must be done prior to running Configure if using the GUI)
  - Build the ALL_BUILD and INSTALL targets in Release mode
* **[libpng](http://www.libpng.org)**
  - Download the zipfile of the source code and extract it
  - Set ZLIB_INCLUDE_DIR to *LIBPACK/include*
  - Set ZLIB_LIBRARY_RELEASE to *LIBPACK/lib/zlib.lib*
  - Set CMAKE_INSTALL_PREFIX to *LIBPACK*
  - Build the ALL_BUILD and INSTALL targets in Release mode
* **[bzip2](https://sourceware.org.bzip2)**
  - Download the source from https://sourceware.org/bzip2/downloads.html
  - Extract it to a directory with no spaces or non-ASCII characters in it
  - Open a Developer's PowerShell
  - `cd X:\path\to\extracted\files`
  - `nmake -f .\makefile.msc`
  - Copy libbz2.lib into *LIBPACK\lib*
  - Copy all header files into *LIBPACK\include*
* **[FreeType](https://freetype.org)**
  - Download the latest version from https://sourceforge.net/projects/freetype/files/freetype2/ (a Windows .zip file is available)
  - Extract the files and run cMake
  - Set HarfBuzz_INCLUDE_DIR to *LIBPACK/include*
  - Set HarfBuzz_LIBRARY to *LIBPACK/lib/harfbuzz.lib*
  - Set ZLIB_INCLUDE_DIR to *LIBPACK/include*
  - Set ZLIB_LIBRARY_RELEASE to *LIBPACK/lib/zlib.lib*
  - Set PNG_INCLUDE_DIR to *LIBPACK/include*
  - Set PNG_LIBRARY_RELEASE to *LIBPACK/lib/libpng16.lib*
  - Set BZIP2_INCLUDE_DIR to *LIBPACK/include*
  - Set BZIP2_LIBRARY_RELEASE to *LIBPACK/lib/libbz2.lib*
  - Set CMAKE_INSTALL_PREFIX to *LIBPACK*
  - Add BUILD_SHARED_LIBS as a boolean value, set to true
  - Add BUILD_STATIC_LIBS as a boolean value, set to true
* **[Tcl/Tk](https://www.tcl.tk)**
  - Download the latest source code for Tcl and Tk
  - Unpack each, and for each
    - Open a Developer's Powershell window and change to the *win* subdirectory in the source download
    - Run `nmake -f makefile.vc release`
    - Run `nmake -f makefile.vc install INSTALLDIR=C:\path\to\libpack`
* **[OpenCASCADE](https://www.opencascade.org)** -- Also known as OCCT, the core CAD kernel used by FreeCAD is fetched from a gitlab mirror that
  provides additional improvements over the upstream version
  - Clone https://gitlab.com/blobfish/occt
  - Run cMake
  - Enable USE_VTK
  - For each of TCL, TK, VTK, and FreeType (represented below by XXX):
    - Set 3RDPARTY_XXX_DIR to *LIBPACK*
    - Set 3RDPARTY_XXX_DLL_DIR to *LIBPACK/bin/*
    - Set 3RDPARTY_XXX_LIBRARY_DIR to *LIBPACK/lib/*
    - Set 3RDPARTY_XXX_INCLUDE_DIR to *LIBPACK/include/* (VTK has an additional path component for the version)
    - Set 3RDPARTY_XXX_LIBRARY to *LIBPACK/lib/xxxYYY.lib* (the name of the library varies, use whatever is present)
  - Set INSTALL_DIR to *LIBPACK*
  - Configure, Generate, and open the project in Visual Studio
  - Switch to Release and build the ALL_BUILD and INSTALL targets
* **[Netgen](https://ngsolve.org)**
  - Clone https://github.com/NGSolve/netgen
  - Switch to the tag you wish to compile (e.g. `git checkout v6.2.2304`)
  - Run cMake
  - Turn off "Superbuild"
  - Turn off "USE_GUI"
  - Set paths for ZLIB, TCL, and TK
  - Set all Python paths in Advanced cMake options (netgen ignores Python_ROOT_DIR)
  - Set CMAKE_INSTALL_PREFIX to *LIBPACK*
  - Switch to Release and build the ALL_BUILD and INSTALL targets
* **[HDF5](https://github.com/HDFGroup/hdf5)**
  - Clone the repository from https://github.com/HDFGroup/hdf5
  - Check out the release tag you want to build (e.g. `hdf5-1_14_1-2`)
  - Deactivate building tests and examples
  - Set ZLIB_INCLUDE_DIR and ZLIB_LIBRARY_RELEASE
  - Set CMAKE_INSTALL_PREFIX to *LIBPACK*
  - Configure and Generate, then open in Visual Studio, switch to Release, and build ALL_BUILD and INSTALL
* **[GMSH](https://gmsh.info)**
  - Download the desired version of the Gmsh source code
  - Set the path to FreeType
  - Set HDF5_DIR to *LIBPACK/cmake*
  - Set HDF5_LIBRARY_RELEASE to *LIBPACK/lib/hdf5.lib*
  - Set HDF5_DIFF_EXECUTABLE to *LIBPACK/bin/hdf5diff.exe*
  - Add a new variable called CMAKE_LIBRARY_PATH and set it to *LIBPACK/win64/vc14/lib* (adapt to your system as needed)
  - Disable OpenMP (as of this writing the compilation fails if it is enabled)
  - Set CMAKE_INSTALL_PREFIX to *LIBPACK*
  - Configure and Generate, then open in Visual Studio, switch to Release, and build ALL_BUILD and INSTALL
* **[PyCXX](https://cxx.sourceforge.net)**
  - Download the latest source from Sourceforge
  - Run `LIBPACK/bin/python.exe setup.py install` from within the extracted directory
* **[ICU](https://icu.unicode.org)** -- Advanced Unicode handling for Xerces-C
  - Clone from https://github.com/unicode-org/icu
  - Switch to the tag you want to build, e.g. `release-73-2`
  - Open the Visual Studio project in icu4c/source/allinone
  - Configure for your platform and Release, and run Rebuild Solution
  - Copy the contents of *bin64/*, *include/*, and *lib64/* to *LIBPACK/bin/*, *LIBPACK/include/* and *LIBPACK/lib*, respectively.
* **[Xerces-C](https://xerces.apache.org/xerces-c/)**
  - Set ICU_INCLUDE_DIR to *LIBPACK/include/unicode*
  - Set CMAKE_INSTALL_PREFIX to *LIBPACK*
