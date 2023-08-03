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

The most common thing to break is the KDE Neon extension, which periodically needs to have its version number updated:
if the snap-building logs contain errors about being unable to locate neon, check for the correct version number at
https://snapcraft.io/docs/kde-neon-extension

Snaps are built based on specific "cores" -- the base operating system whose libraries they use. From time to time
we have to update to a new core. The current builds use "core20", or Ubuntu 20.04 LTS. Updating the core will likely
require updating many of the other version numbers in the snap YAML file.

## Important links

  * https://snapcraft.io/freecad
  * https://ubuntu.com/tutorials/create-your-first-snap
