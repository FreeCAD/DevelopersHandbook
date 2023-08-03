---
layout: default
---

# Building release packages

This page describes how to build FreeCAD packages for the different supported platforms.

## Windows Installer

The main entry point to build a Windows installer is within the FreeCAD sources, at https://github.com/FreeCAD/FreeCAD/tree/master/src/WindowsInstaller . To build the installer:

### Creating a Windows installer for FreeCAD

These are instructions for building an NSIS-based installer for FreeCAD. They were designed for FreeCAD 0.21 and later,
and presume that you have cloned a copy of FreeCAD's source code, and therefore have the directory *src/WindowsInstaller*.

#### Install NSIS
To set up your system for building an NSIS installer:
1. Install the latest version 3.x of NSIS (https://nsis.sourceforge.io/Download)
2. Download these special release files of NSIS that support large strings:</br>
   https://nsis.sourceforge.io/Special_Builds#Large_strings</br>
   and copy the contained files into the corresponding NSIS installation folders
3. Download these special release files of NSIS that support logging:</br>
   https://nsis.sourceforge.io/Special_Builds#Advanced_logging</br>
   and copy the contained files into the corresponding NSIS installation folders
4. Download and install the nsProcess plugin from https://nsis.sourceforge.io/NsProcess_plugin -- you will need the version that supports Unicode, so make sure to follow the appropriate instructions on their site to install that one (as of this writing it involves manually copying and renaming the plugin DLL file).

#### Build the installer
Next, update the installer settings for the current version of FreeCAD. Starting from the *src/WindowsInstaller* folder in the FreeCAD source tree:
1. Set the appropriate version strings for the release you are creating. These are used to construct the filename of the installer, among other things. If you have to upload a new version of the installer for the exact same release of FreeCAD, increment `APP_VERSION BUILD` as needed.
```
!define APP_VERSION_MAJOR 0
!define APP_VERSION_MINOR 21
!define APP_VERSION_REVISION 0
!define APP_VERSION_EMERGENCY "RC1"
!define APP_VERSION_BUILD 1
```
2. Within the folder *src/WindowsInstaller*, create a new folder called MSVCRedist and copy the following files from your MSVC installation into it:
```
vcruntime140.dll
concrt140.dll
msvcp140.dll
vcamp140.dll
vccorlib140.dll
vcomp140.dll
```    
3. Open the file *Settings.nsh* with a text editor (both jEdit and Visual Studio Code are good editors for NSIS files). Edit the following paths to correspond to your system: `FILES_FREECAD` corresponds to your installation directory (e.g. `CMAKE_INSTALL_PREFIX` if you self-compiled) and `FILES_DEPS` is the folder you created with the MSVC redistributable files in it.
```
!define FILES_FREECAD "C:\FreeCAD\Installer\FreeCAD"
!define FILES_DEPS "C:\FreeCAD\Installer\MSVCRedist"
```
4. Ensure the FreeCAD files are in place. Here you have two options:
   * If you are working from an already-compiled version of FreeCAD provided to you by an outside source: in this case, simply ensure that `FILES_FREECAD` is set to the directory containing those files.
   * If you compiled FreeCAD on your own as described [here](https://wiki.freecad.org/Compile_on_Windows) (and using the Install option outlined there). Then:
       * Open the file *Settings.nsh* as described in step 3. above and set there</br>
        `!define FILES_FREECAD` to the folder you specified as `CMAKE_INSTALL_PREFIX`
       * Copy into that folder the file *Delete.bat* that is part of the installer
       * open a command line in Windows and change to the folder
       * run the comamand</br>
        `Delete.bat`
       * (These steps assure that the installer only contains files users need. Moreover it assures that the
       overall files size is below 2 GB and we can use the most compact compression for the installer.)
5. Right-click on the file *FreeCAD-installer.nsi* and choose **Compile NSIS script**
   to compile the installer.


NOTE: For test builds of the installer you can turn off the LZMA compression and use the much faster default compression. This speeds up
the build time for the installer but significantly increases the final installer file size. The LZMA compression is turned off by commenting 
out the line
```
!SetCompressor lzma
```
in the file *Settings.nsh*.


## Conda builds for Linux, Mac, and Windows

The same system that builds the "Development Weekly" builds is used to build the release versions of the Linux AppImages, Mac app bundles, and a standalone Windows executable (e.g. a non-installer version).

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
