---
layout: default
---

# Stages

Each release goes through the following milestones and stages:

## Announcement Preparation

A release may be triggered for several reasons.
- An event or dependency in the development roadmap may require a release.
- There may be a desire to have a major release included in a major platform release like the next version of Ubuntu.
- Or it may be that a significant set of features has matured to the point that they are ready for general usage.

Whatever the reason, the first step in the determination by the maintainers that a release is needed.

The maintainers will create a Github project to track the release.
The maintainers will schedule a meeting to review outstanding Pull Requests. 


## Announcment milestone

Next, the maintainers will make a formal announcment on the forum. The announcement will layout the expected timeline for the release and call for final submissions.  The announcment should be further communicated through official and unofficial channels like Twitter and Reddit.

## Final Feature Submission Stage

The purpose of the announcment is to let contributors know that a feature freeze is imminent. Any work in process should be finalized and a Pull Request submitted.

At this point, a PR may not be ready to merge and can be marked as _draft_.  Creating the PR makes the maintainer community aware of pending changes that the contributor would like to have considered for the release. The maintainers will allow time (typically 1-2 weeks) for Pull Requests to be submitted.

The announcment should include a date for the PR Review meeting.

The announcment should also announce a splash-screen contest and provide a forum thread for submissions.

## PR Review Milestone

After the Submission Stage is closed, the maintainers will review all outstanding Pull Requests. For each one, they will determine if the feature is sufficiently mature to include in the release. Specifically they will consider:

- How significant is the feature
- How likely is the feature to negatively impact user workflow
- How well documented is the new feature
- How much additional testing and stabilization is likely required

Maintainers will strive to include as many mature features as possible while minimizing the time needed to resolve bugs, complete documentation, and stabilize the release.

Each PR will be tagged with the current release if it is to be included.

Following the PR Review, any new feature Pull Requests will be held for a later version.  Contributors should adjust their expectations about code reviews.

## PR Merge Phase

During this phase, development focus will turn towards merging the remaining tagged outstanding PRs.
After the PR Merge phase, only bug-fix PRs will be merged.

## Release Candidates

After all outstanding PRs are merged and all critical bugs are resolved, a Release Candidate (RC) version will be created and a public announcment made.  This is a request to the community for intentional and deliberate testing with the goal of uncovering as many bugs as possible.

Translation files will be pushed to CrowdIn and a public call for translation assistance should be made.

Release notes should be reviewed at this point and any ommisions corrected

Numerous release candidates may be produced as critical bugs are addressed.

## Final Release

- The result of the splash screen contest is announced and the new splash screen is added to the source.

- update all versions in a number of source code locations:
    - README.md
    - appveyor.yml
    - CMakeLists.txt
    - src/Tools/offlinedoc/buildqhelp.py
    - src/XDGData/org.freecadweb.FreeCAD.appdata.xml
    - vagrant/Xenial/generate_yaml.sh
    - vagrant/Xenial/bin/launcher
    - vagrant/generate_yaml.sh
    - vagrant/FreeCAD.sh

    - Prepare a packager's release files and alert the packagers.

- Tag the new release
        TODO: relevant git commands

## Release Announcment

Todo
