---
layout: default
---


# Next Release

Releasing an official 1.0 release is a major milestone as well as a psychologically important event for the project. It is a signal to the world that FreeCAD is ready for production use, and that it is a mature, stable, and reliable tool for the job.

The criteria for when FreeCAD would be ready for 1.0 status has been debated for a long time. In general, the consensus has been that the software should be both feature complete and stable.  Feature completeness means that it has the major features a user would expect from a mature CAD application, not that it has ALL the features we desire or can imagine.

Stability means that the software does not crash routinely and that user models are not corrupted by normal use.  It does not mean that the software is bug free, or that it is perfect.  It does mean that the software is reliable enough to be used in production, and that it is not likely to cause data loss or other major problems.

At this time, we are setting four major goals to be completed for version 1.0.  When 1.0 is released, it will likely contain many more features and improvements than these four but we will not release 1.0 until these four are achieved.

 - Mitigating the topological naming problem
 - An integrated assembly workbench
 - A unified Material system
 - Improved initial user experience

## Topological Naming Problem

The toponaming problem has been widely discussed and work is proceeding to resolve it.

### Reference
 - [original issue](https://github.com/FreeCAD/FreeCAD/issues/8432)
 - [Github Project](https://github.com/orgs/FreeCAD/projects/2)



## Assembly Workbench

Building multi-part assemblies is something users expect a modern CAD system to do.  FreeCAD has multiple add-on options but lacks an integrated Assembly workbench.  Version 1.0 will address this deficiency.  The integrated workbench delivered in 1.0 will be minimally viable.  It will provide the core functionality users need but will likely be less capable than the addon options currently available.  Goals for this release are modest and include
 - An intuitive and user-friendly user interface
 - An integrated solver to resolve assembly constraints
 - A standard API for assemblies that external workbench authors can use to be compatible with both that workbench and each other.

### Reference
 - [Ondsel Blog Series](https://ondsel.com/blog/default-assembly-workbench-1/)
 - [Github Project](https://github.com/orgs/FreeCAD/projects/7)


## Material System

The existing material capability is limited and does not meet the needs of many users.  Version 1.0 will start the process of improving the material system. A new material system will address needs in many different workbenches include FEM, Arch, Path, and Render.  Like the assembly workbench, goals for 1.0 are modest. We will provide minimum viable functionality with a solid foundation for future improvment.

### Reference
 - [original issue](https://github.com/FreeCAD/FreeCAD/issues/10174)
 - [Github Project]()
 - [Ondsel Blog post](https://ondsel.com/blog/freecad-needs-a-better-materials-system)
 - [Forum topic](https://forum.freecad.org/viewtopic.php?style=4&t=78242)


## Initial User Experience

FreeCAD is often criticized as being hard to learn.  This is understandable because it is a complex application with many features.  However, we can do better, especially for first-time users who struggle to get started.

The goals for 1.0 are

- A First Run Wizard
- Improved Start Page
- UI / UX improvements to improve discoverability and usability.

### Reference
 - [Github Project]()
