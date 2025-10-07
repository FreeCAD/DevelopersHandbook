---
layout: default
---

# Design Guide

**This Design Guide is organized into the following sections**

- [History and Purpose](./history-and-purpose)
- [Glossary of Terms](./glossary)
- [Principles](./principles)
- [Zones](./zones)
- [Primary Elements](./elements)
- [Input Hints Guidelines](./input-hints)
- Interactive Controls (Still under development)
- [Naming Convention](./naming)
- Colors and Accessibility (Still under development)
- Theme/Styling (Still under development)
- Icons/Art (Still under development)
- [FreeCAD Brand Guidelines](./logo)

*The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119.txt).*

## How to use this guide

This guide is organized into multiple documents as outlined above, for the purpose of aiding developers in finding relevant content without excess information. Newcomers to FreeCAD, and this Design Guide, should familiarize themselves with the principles and assumptions which were used as the foundation on which these guidelines were built before progressing to more targeted content. The rest of the sections are organized in a manner of logical hierarchy of concepts and guidelines starting with more conceptual matter and moving on towards greater levels of detail.

## Assumptions:

The interface is tailored around a 1920 x 1080 standard resolution.

According to the [*Steam monthly hardware survey*](https://store.steampowered.com/hwsurvey/Steam-Hardware-Software-Survey-Welcome-to-Steam), over 90% of all computer displays utilize 1920x1080 or higher resolution. The user interface for FreeCAD shall be optimized for this recommended resolution at 100% scaling. This shall be the "reference resolution" for the remainder of this design guide.

**Note:** For operating system specific elements Windows is considered the standard due to market share (70% in 2023). Every effort should be made to maintain a uniform appearance across platforms.

**Note:** Deviations from the prescriptive guidelines in this design document require justification utilizing sound reasoning and require a design review that such deviation(s) are both warranted and fit within the needs of the user without degrading the overall consistency of the user experience.
