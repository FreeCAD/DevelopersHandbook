---
layout: default
---

Between major releases, bugs are occasionally found and repaired. These fixes may be urgent enough or isolated enough that applying them incrementally to the existing ‘stable’ version is both possible and desirable. A set of such changes may be grouped together and released as a ‘minor’ version.

Usually these fixes have already been applied to the development branch.  The process of applying them to the previous stable version is called 'backporting'

## Stages

Each minor release goes through the following stages:

## Creation of Release Milestone

The need for a minor release is triggered by a PR or commit which is deemed worth backporting to the stable version.  The consideration is based on a couple factors

- Does the bug result in the loss of data under normal circumstances?
- Does the bug expose user data or present the possibility of user data being
  exposed?
- Does the bug create a significant inconvenience for the user?

When a PR or commit is found worth backporting, a maintainer will create a new issue. The issue should have suitable title like  [BACKPORT REQUEST] <some description>

The issue should link to the PR in question.

The issue should be labeled with a Milestone label for the next minor release.  If the milestone label doesn't exist, the maintainer will create it.

## Create a backport PR

A maintainer will create a new PR targeting the stable branch.  The relevant commits can be cherry-picked and added to the new PR.

The backport PR will go through the same process as any other PR.

## Creating the Minor version release

Minor bugs may be gathered together for a period of days or weeks to batch them together. A significant bug or one that risks user data may require an expedited release.

When the determination is made to proceed with the release, the following things happen"

1. something
2. Something else
