---
layout: default
---

# Dependencies

Information about the dependencies in FreeCAD

## Overview

Like most software projects, FreeCAD depends on many other packages. Most critical among these is [OpenCASCADE](https://www.opencascade.com/),
the actual CAD kernel that FreeCAD is built upon. Of course, OpenCASCADE itself has its own dependencies. Many of which have *their* own
dependencies, etc. The dependency graph quickly become overwhelming if you have to manually install each one. This is where package managers come
in. Depending on what platform you are developing on, you have a variety of tools at your disposal to resolve these dependencies. A brief overview
of the most common ones is included below. The main purpose of this document is to highlight the primary dependencies and describe what they do and
why FreeCAD needs them.

## Major dependencies

The following is a brief overview of what each of FreeCAD's most critical 3rd-party library dependencies provide.

### OpenCASCADE

OpenCASCADE Technology (OCCT) is a full-featured, professional grade CAD kernel. It was developed in 1993 and originally called
CAS.CADE, by Matra Datavision in France for the Strim (Styler) and Euclid Quantum applications. In 1999 it was released as open
source software, and since then it's been called OpenCASCADE.

OCCT is a big and complex set of C++ libraries that provide functionality required by a CAD application:
* A complete STEP compliant geometry kernel.
* A topological data model and needed functions to work with shapes (cut, fuse, extrude, and many others).
* Standard import and export processors for files like STEP, IGES, VRML.
* A 2D and 3D viewer with selection support.
* A document and project data structure with support for save and restore, external linking of documents, recalculation of design history (parametric modeling) and a facility to load new data types as an extension package dynamically.

Note that the "Community Edition" of OpenCASCADE is no longer supported by FreeCAD, you must install the first-party library.

### Python

Python is a popular all-purpose scripting language that is widely used in Linux and open source software.
In FreeCAD, Python is used during compilation and also at runtime in different ways. FreeCAD itself contains
hundreds of thousands of lines of Python code, and all FreeCAD Addons and Macros are written in Python.

### Qt

Qt is one of the most popular graphical user interface (GUI) toolkits available in the open source world.
FreeCAD uses this toolkit to draw the interface of the program, as well as to provide some advanced networking functionality
and translation facilities. FreeCAD currently provides primary support for Qt v5.12 - v5.15, and work is ongoing to support Qt v6.4 and
later.

Further information about Qt libraries and their programming documentation are available at [Qt Documentation](https://doc.qt.io/).

### PySide and Shiboken

Shiboken is the Python binding generator that Qt for Python uses to create the PySide module, in other words, it is the system that
is used to expose the Qt C++ API to the Python language. FreeCAD's Python code uses the PySide library to draw its user interface elements.
FreeCAD provides primary support for PySide2 (corresponding to Qt5), and support for PySide6 (corresponding to Qt6) is in progress.
Python developers should import `PySide` (with no numeric suffix) -- at compile-time FreeCAD's build system will generate the appropriate
version.

### Coin3D

The dependencies Coin, Quarter, and Pivy are collectively part of the Coin3D library system. Coin3D is a high-level 3D graphics library
with a C++ application programming interface. It uses scenegraph data structures to render real-time graphics suitable for all kinds
of scientific and engineering visualization applications.

Coin3D (Open Inventor) is used as the 3D viewer in FreeCAD because the OpenCASCADE viewer (AIS and Graphics3D) has limitations and performance
bottlenecks, especially with large-scale engineering rendering; other things like textures or volumetric rendering are not entirely supported
by the OpenCASCADE viewer.

### Boost

The Boost C++ libraries are collections of peer-reviewed, open source libraries that extend the functionality of C++. They are intended to
be widely useful across a broad spectrum of applications, and to work well with the C++ Standard Library. The Boost license is designed
to encourage their use in both open source and closed source projects. Over time, many Boost libraries end up incorporated into the C++ standard
library itself. As this happens, developers should work to transition their code to the `std::` variant rather than the `boost::` variant. Note
that exception is made for `boost::regex`, which as of this writing is still considerably faster than its `std::regex` counterpart.

### Xerces-C++

Xerces-C++ is a validating XML parser written in a portable subset of C++. Xerces-C++ makes it easy to give your application the ability to
read and write XML data. A shared library is provided for parsing, generating, manipulating, and validating XML documents. Xerces-C++ is
faithful to the XML 1.0 recommendation and associated standards. The parser is used for saving and restoring parameters in FreeCAD.

### Eigen3

Eigen is a C++ template library for linear algebra: matrices, vectors, numerical solvers, and related algorithms. If you just want to use Eigen,
you can use the header files right away. There is no binary library to link to, and no configured header file. Eigen is a pure template library
defined in the headers. Eigen is used in FreeCAD for many vector operations in 3D space.

### Zipios++

Zipios++ is a C++ library for reading and writing .zip files. Access to individual entries is provided through standard C++ iostreams. A
simple read-only virtual file system that mounts regular directories and .zip files is also provided. The structure and public interface
of Zipios++ are loosely based on the java.util.zip package of Java.

FreeCAD's native file format .FCstd is in reality a .zip file that stores and compresses other types of data within it, such as BREP
and XML files. Therefore, Zipios++ is used to save and open compressed archives, including FreeCAD files.

A copy of Zipios++ is included in the source code of FreeCAD so it is compiled together with it. If you want to use an external Zipios++
library, provided by your operating system, you may set `-DFREECAD_USE_EXTERNAL_ZIPIOS=ON` with cmake.

Zipios++ uses the Zlib library to perform the actual decompression of files.

### Zlib

Zlib is designed to be a free, general-purpose, lossless data-compression library for use on any computer hardware and operating system.
It implements the DEFLATE compression algorithm commonly used in .zip and .gzip files.

A copy of this library is included in the source code of FreeCAD so it is compiled together with it.

## Dependency management

Each OS uses different mechanisms to supply dependencies. These mechanisms are typically designed such that installing a single dependency
also automatically installs all of *its* dependencies, eliminating the need for you to manually install every single dependency by hand.

### Linux

As a general rule in Linux, you should use your system's package manager to install dependencies, including Python libraries, rather than using `pip`.
Unfortunately, different distributions use different names for the various packages, so it is sometime challenging to determine exactly what needs
to be installed. It is very common to have to adjust the one-line command below for your system by removing failing packages and locating alternates
(usually provided under a different name).

A single command to install the dependencies on Ubuntu-based systems is:
```
sudo apt install cmake cmake-gui libboost-date-time-dev libboost-dev libboost-filesystem-dev libboost-graph-dev libboost-iostreams-dev libboost-program-options-dev libboost-python-dev libboost-regex-dev libboost-serialization-dev libboost-thread-dev libcoin-dev libeigen3-dev libgts-bin libgts-dev libkdtree++-dev libmedc-dev libocct-data-exchange-dev libocct-ocaf-dev libocct-visualization-dev libopencv-dev libproj-dev libpyside2-dev libqt5opengl5-dev libqt5svg5-dev qtwebengine5-dev libqt5x11extras5-dev libqt5xmlpatterns5-dev libshiboken2-dev libspnav-dev libvtk7-dev libx11-dev libxerces-c-dev libzipios++-dev occt-draw pyside2-tools python3-dev python3-matplotlib python3-packaging python3-pivy python3-ply python3-pyside2.qtcore python3-pyside2.qtgui python3-pyside2.qtsvg python3-pyside2.qtwidgets python3-pyside2.qtnetwork python3-pyside2.qtwebengine python3-pyside2.qtwebenginecore python3-pyside2.qtwebenginewidgets python3-pyside2.qtwebchannel python3-markdown python3-git python3-pyside2uic qtbase5-dev qttools5-dev swig
```
To install dependencies on Fedora systems (Derived from the [RPM spec](https://src.fedoraproject.org/rpms/freecad)):
```
sudo dnf install clang cmake gcc-c++ gettext dos2unix doxygen swig graphviz gcc-gfortran desktop-file-utils freeimage-devel libXmu-devel mesa-libGL-devel mesa-libGLU-devel libglvnd-devel opencascade-devel Coin4-devel python3-devel python3-matplotlib python3-pivy boost-devel eigen3-devel tbb-devel qt5-qtbase-devel qt5-qtsvg-devel qt5-qtxmlpatterns-devel qt5-qttools-devel qt5-qttools-static qt5-qtwebengine-devel xerces-c xerces-c-devel libspnav-devel python3-shiboken2-devel python3-pyside2-devel pyside2-tools smesh-devel zipios++-devel python3-pycxx-devel libicu-devel vtk-devel med-devel libkdtree++-devel libappstream-glib python3-pivy python3-matplotlib python3-collada python3-pyside2 qt5-assistant
```

### Windows

By far the easiest way to install the dependencies for Windows is to download the pre-packaged "LibPack" from https://github.com/FreeCAD/FreeCAD-LibPack/releases. This
download provides a single source for all of FreeCAD's library dependencies.

### Mac OS

Mac OS does not have a built-in package management system, but it is possible to use [Homebrew](https://brew.sh) to install FreeCAD's dependencies. Note however that
this can be a very challenging process because Homebrew often updates libraries to new versions before downstream packages are updated to accomodate them:
it is usually necessary for a developer to manually extract an old dependency for installation as a custom "tap".

An alternative is to use Conda to create your Mac OS build system. Documentation of this process is a [work-in-progress by several users on the FreeCAD Forum](https://forum.freecad.org/viewtopic.php?t=70038).
