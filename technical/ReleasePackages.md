---
layout: default
---

# Building release packages

This page describes how to build FreeCAD installer packages for the different supported platforms.

## Windows

### Installer

The main entry point to build a Windows installer is within the FreeCAD sources, at https://github.com/FreeCAD/FreeCAD/tree/master/src/WindowsInstaller . To build the installer:

1. Open the file *Settings.nsh* with a text editor and adapt there the following paths to the ones on your machine, e.g.: `!define FILES_FREECAD "C:\FreeCAD\Installer\FreeCAD"` and `!define FILES_DEPS "C:\FreeCAD\Installer\MSVCRedist"`
2. If you want to build a 32bit installer, comment out `!define MULTIUSER_USE_PROGRAMFILES64` in *Settings.nsh*
3. Install the latest version 3.x of NSIS from [NSID Download page](https://nsis.sourceforge.io/Download)
4. Download a special release files of NSIS that support large strings from [NSIS Special Builds](https://nsis.sourceforge.io/Special_Builds#Large_strings) and copy the containing files into the corresponding NSIS installation folder
5. Download a special release files of NSIS that support logging from [NSIS Special Builds](https://nsis.sourceforge.io/Special_Builds#Advanced_logging)  and copy the containing files into the corresponding NSIS installation folder
6. Copy the file *~\nsprocess\Include\nsProcess.nsh* to the *\Include* folder of NSIS's installation folder
7. Copy the file *~\nsprocess\Plugins\x86-unicode\nsProcess.dll* to the *\Plugins\x86-unicode* folder of NSIS's installation folder. You can alternatively get nsProcess from [NsProcess plugin - NSIS](https://nsis.sourceforge.io/NsProcess_plugin).

Now you have 2 options:

- Either you got an already compiled FreeCAD, then copy all FreeCAD files to the *~\FreeCAD* folder   e.g. *C:\FreeCAD\Installer\FreeCAD*
- Or you compiled FreeCAD on your own as described [on the FreeCAD wiki](https://wiki.freecad.org/Compile_on_Windows). Then:
  - Open the file *Settings.nsh* as described in step 3. above and set `!define FILES_FREECAD` to the folder you specified as `CMAKE_INSTALL_PREFIX`
  - Copy into that folder the file *Delete.bat* that is part of the installer
  - Change to that folder and search it for ***_debug.***
  - Delete all found files
  - Repeat this for the searches with ***_d.***, ***.pyc** and ***.pdb**
  - Open a command line in Windows and navigate to the FreeCAD folder
  - run the comamand  `Delete.bat`
  
The above steps assure that the installer only contains files users need. Moreover it assures that the overall files size is below 2 GB and we can use the most compact compression for the installer.

8. If you use a version of FreeCAD that was compiled using another MSVC version than MSVC 2019, copy its distributable DLLs to the folder FILES_DEPS (see step 1 above).
9. Right-click on the file *FreeCAD-installer.nsi* and choose **Compile NSIS script** to compile the installer.
10. The folder *~\MSVCRedist* contains already all MSVC 2019 x64 redistributable DLLs necessary for FreeCAD. If another MSVC version was used to compile FreeCAD, replace the DLLs by the ones of the used MSVC. (They are usually available in the folder *C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC\Redist\MSVC*)

For test builds of the installer you can turn off the compression. This speeds up the build time for the installer a lot but increases its file size. The compression is turned off by uncommenting the line `SetCompressor /SOLID lzma` in the file *Settings.nsh*.

### Conda

**To be expanded**

It is also possible to build a FreeCAD package for Windows with [conda](https://conda.io). This is produced as a .7z package that can be unzipped and run directly, without installation. The scripts needed to produce such a package are located in a separate Git repository at [FreeCAD/FreeCAD-Bundle](https://github.com/FreeCAD/FreeCAD-Bundle). The main script to run is [conda/win/create_bundle.bat](https://github.com/FreeCAD/FreeCAD-Bundle/blob/master/conda/win/create_bundle.bat).

You will need:

1. **Mambaforge** from [GitHub - conda-forge/miniforge: A conda-forge distribution.](https://github.com/conda-forge/miniforge#mambaforge)
2. **7zip** from https://www.7-zip.org/

To use the script:

1. TODO document how to install Mambaforge
2. Clone the FreeCAD-Bundle repository
3. Run the create_bundle.bat script

## MacOS

### Conda

**To be expanded**

MacOS packages are also built with [conda](https://conda.io).

## Linux

### Conda

On Linux, we mostly let distributions produce packages themselves from the FreeCAD source code. However, we also produce an [AppImage](https://appimage.org/) package, that runs on (hopefully) all Linux distributions. AppImages achieve that by embedding the largest part of the libraries FreeCAD depends on, and rely only on a few very standard system libraries.

The most complex part, when building an AppImage, is therefore to gather those libraries. FreeCAD therefor uses [conda](https://conda.io) which is at the same time a build system and a package manager, to locate, download and organize these libraries. After this is done, the official AppImage tool is used to actually build the AppImage. Note that FreeCAD itself is built and made available on conda, so the process of building an AppImage does not compile FreeCAD. It rather uses a compiled version of FreeCAD already available on the conda platform.

The main structure used to gather FreeCAD and its dependencies and build appimages is located in a separate Git repository at [FreeCAD/FreeCAD-Bundle](https://github.com/FreeCAD/FreeCAD-Bundle). The main script to run is [conda/linux/create_bundle.sh](https://github.com/FreeCAD/FreeCAD-Bundle/blob/master/conda/linux/create_bundle.sh). This script will do all the process of assembling and producing an AppImage.

You will need:

1. **Mambaforge** from https://github.com/conda-forge/miniforge#mambaforge 
2. **appimagetool** from https://appimage.github.io/appimagetool/

To use the script:

1. Clone the FreeCAD-Bundle repository
2. Download and place the appimagetool Appimage in the root of the repo, make it executable with `chmod +x`
3. If you chose to not install the conda environment when installing Mambaforge, you will need to enable a conda environment now. Do it by running the following in your terminal: `eval "$(/$USER/Mambaforge/bin/conda shell.sh hook)"` (replace the Mambaforge path by yours if you installed it somewhere else and sh by your shell name if not sh). This will make the **conda** and **mamba** commands available in this terminal session
4. Run `create_bundle.sh`

Note that appimages produced by that script are by default not signed. It is generally seen on the AppImage website as not very widely used to sign AppImages. If you wish to sign the package, add `--sign-key you@yourdomain.com` to the appimage line in the create_bundle.sh script. You will need to have a gpg key configured for that email.
