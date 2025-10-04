---
layout: default
---

# Maintaining the Snap package

FreeCAD is provided as a Snap package in multiple versions:
  * stable -- built from the latest full release
  * beta -- built weekly from master
  * edge -- built daily from master
  * freecad-realthunder -- a convenience build of the LinkStage3 branch

Each of these is represented by a branch at https://github.com/FreeCAD/FreeCAD-snap -- to make changes,edit [the snapcraft.yaml file](https://github.com/FreeCAD/FreeCAD-snap/blob/master/snap/snapcraft.yaml) on the branch you want to update.

GitHub actions are used to do the actual building of each of these branches.

## Routine maintenance

The most common thing to break is the KDE Neon extension, which periodically needs to reference an update version of
the kde-frameworks (e.g. kde-frameworks-5-99-qt-5-15-7-core20):
if the snap-building logs contain errors about being unable to locate neon, check for the correct version number at
https://snapcraft.io/docs/kde-neon-extension and update it everywhere it appears in the snapcraft.yaml file.

Snaps are built based on specific "cores" -- the base operating system whose libraries they use. From time to time
we have to update to a new core. The current builds use "core22", or Ubuntu 22.04 LTS. Updating the core will likely
require updating many of the other version numbers in the snap YAML file.

## Dependencies

Several of FreeCAD's dependencies take a long time to build and seldomly change: the two largest culprits here are
OCCT and GMSH. These two packages have been split into their own "unlisted" snap, called freecad-deps-core22. To
update the versions of those libraries, that snap is the one that must be updated, rather than freecad-snap itself.

## Important links

  * https://snapcraft.io/freecad
  * https://ubuntu.com/tutorials/create-your-first-snap
