---
layout: default
---

# Maintainers Guide

Guidelines for Maintainers regarding code review and merge procedures.  Git can be [configured to make checking out and reviewing PRs easier](./git).

## The Team

This is the list of **current** maintainers & admins.

### Maintainers

-   [![GitHub]][GitHub-AdrianInsAval]
    [![Forum]][Forum-AdrianInsAval]
    `Adrián Insaurralde Avalos`

-   [![GitHub]][GitHub-Hyarion]
    [![Forum]][Forum-Hyarion]
    `Benjamin Nauck`

-   [![GitHub]][GitHub-Kadet]
    [![Forum]][Forum-Kadet]
    `Kacper Donat`

-   [![GitHub]][GitHub-Roy]
    [![Forum]][Forum-Roy]
    `Roy-043`

-   [![GitHub]][GitHub-WandererFan]
    [![Forum]][Forum-WandererFan]
    `WandererFan`

### Admins

-   [![GitHub]][GitHub-Sliptonic]
    [![Forum]][Forum-Sliptonic]
    `Brad Collette`

-   [![GitHub]][GitHub-Chennes]
    [![Forum]][Forum-Chennes]
    `Chris Hennes`

-   [![GitHub]][GitHub-WMayer]
    [![Forum]][Forum-WMayer]
    `Werner Mayer`

-   [![GitHub]][GitHub-Yorik]
    [![Forum]][Forum-Yorik]
    `Yorik van Havre`


[GitHub-AdrianInsAval]: https://github.com/adrianinsaval 'GitHub'
[GitHub-WandererFan]: https://github.com/WandererFan 'GitHub'
[GitHub-Sliptonic]: https://github.com/sliptonic 'GitHub'
[GitHub-Hyarion]: https://github.com/hyarion 'GitHub'
[GitHub-Chennes]: https://github.com/chennes 'GitHub'
[GitHub-WMayer]: https://github.com/wwmayer 'GitHub'
[GitHub-Yorik]: https://github.com/yorikvanhavre 'GitHub'
[GitHub-Kadet]: https://github.com/kadet1090 'GitHub'
[GitHub-Roy]: https://github.com/Roy-043 'GitHub'

[Forum-AdrianInsAval]: https://forum.freecad.org/memberlist.php?mode=viewprofile&u=19302 'Forum'
[Forum-WandererFan]: https://forum.freecad.org/memberlist.php?mode=viewprofile&u=1375 'Forum'
[Forum-Sliptonic]: https://forum.freecad.org/memberlist.php?mode=viewprofile&u=708 'Forum'
[Forum-Hyarion]: https://forum.freecad.org/memberlist.php?mode=viewprofile&u=34430 'Forum'
[Forum-Chennes]: https://forum.freecad.org/memberlist.php?mode=viewprofile&u=11959 'Forum'
[Forum-WMayer]: https://forum.freecad.org/memberlist.php?mode=viewprofile&u=69 'Forum'
[Forum-Kadet]: https://forum.freecad.org/memberlist.php?mode=viewprofile&u=60296 'Forum'
[Forum-Yorik]: https://forum.freecad.org/memberlist.php?mode=viewprofile&u=68 'Forum'
[Forum-Roy]: https://forum.freecad.org/memberlist.php?mode=viewprofile&u=22936 'Forum'

[GitHub]: https://img.shields.io/badge/-181717?style=flat&logo=GitHub&logoColor=white
[Forum]: https://img.shields.io/badge/-418FDE?style=flat&logo=FreeCAD&logoColor=white


## The Role of a Maintainer

The maintainers group has the ultimate responsibility for determining which contributions are accepted into the FreeCAD source. To perform their duty, maintainers have elevated permission.   Maintainers use this authority within the scope and process outlined by the [CONTRIBUTING.md](https://github.com/FreeCAD/FreeCAD/blob/master/CONTRIBUTING.md) document.

## CONTRIBUTING.md

This document describes the expected process to go from an Issue to a merged piece of code. All Maintainers should be familiar with this document, and work to follow the process that it outlines. In particular, once an Issue has an agreed-upon solution path, then a PR that conforms to that path should be merged without further debate about the merits of the solution: code review focuses solely on whether the PR does indeed follow the agreed-upon path, and on identifying major defects in the code. Minor defects may be included in the review process, but should not prevent the PR from being merged in a timely manner.

## Maintainer Duties

Individual maintainers have two primary duties:
 - They participate in Issue discussions to help clarify the Issue and guide the discussion towards a viable solution path. This includes refining, splitting, closing duplicates, and ensuring that all discussion participants are professional, inclusive, and working towards a common goal of improving FreeCAD.
 - Once an Issue has been identified and agreed upon and a PR has been submitted to resolve it, Maintainers participate in the PR discussion to move it towards merge as quickly as possible.  This includes reviewing the code to identify major defects and conflicts with FreeCAD's coding guidelines. Maintainers also work to further the conversation around the PR by clarifying open questions, involving others, and drawing attention to critical concerns. If the PR is by a new Contributor, Maintainers work to onboard that Contributor, ensuring they understand the process, feel welcome, and receive the mentorship they need to succeed.

All open PRs should have at least one maintainer identified in the 'Assignees' field.  PRs should not be assigned to a maintainer. Instead, maintainers should self-assign PRs. If a PR does not have an assigned maintainer in a reasonable time, the author may comment and ask for a maintainer.

## Code Review Process

Anyone who wishes to participate in the code review process may do so, but all participants should strive to uphold the tenets of CONTRIBUTING.md -- Maintainers are responsible for guiding the review process, and should assist in keeping the discussions on-task. Important qualities to evaluate during a code review are:
1. Does the coded solution conform to the agreed-upon path in the Issue that the PR addresses?
2. Does the code compile on all platforms?
3. Does the code pass the CI test suite?
4. Does the code include new tests to validate its functionality? Note that we do not have a firm requirement that tests be provided, but we should strive to do so when possible.
5. Is the code written in a reasonably easy-to-understand manner, following modern best-practices for the language it is written in? Not all "best practices" violations represent major defects, but care should be taken to guide Contributors toward submitting the best code they can. Merging code with minor best-practices defects and advising Contributors to address those defects in follow-on PRs is the encouraged workflow.
6. Are the commits well-structured, with clear commit messages following the guidelines outlined in CONTRIBUTING.md?
7. Are there any copyright issues that must be addressed? (e.g. use of logos or graphics from other organizations, code copied from other projects, etc.) Any copyrighted content (logos, graphics, code, etc.) included in FreeCAD must have a compatible license, and should have clear permission for us to use it. Of course, it must also be used in a way that's beneficial to FreeCAD's users.

### When is it "good enough"?

The most difficult (and divisive) judgement call a Maintainer must make is, when is imperfect code good enough to merge anyway? In any significant block of code, it's nearly always possible to identify items that could be improved upon. A Maintainer must decide when it is appropriate to merge code, even if they feel that improvements can be made to it. The judgement rests on several criteria:
1. Does the PR fix an important bug? If it does, then getting the fix into the codebase quickly *may* take precedence over any other factor.
2. Is the code blatantly bad by modern software development standards? For example, in C++, does the code use C-style loops, raw pointers, unmanaged memory allocations, etc.? In Python does it eschew PEP8 coding conventions, use ranges in for loops for simple array access, or fail to handle UTF-8 string conversions? If so, the "code smell" may be so bad that it should not be included, even if it does solve some problem. The infinite-horizon maintainability of the codebase is more important that the immediate fix at hand.
3. Is the developer new to FreeCAD? If so, Maintainers should be particularly aware that Open Source development is a new experience for those just joining us, and the encouragement of having a PR merged with suggestions for future improvements may be worth the short-term hassle of managing a multi-PR workflow.
4. Is the developer combative, or resistant to even simple change suggestions? Community-building is a critically important part of Open Source software development, and occasionally that means actively resisting destructive contributors.

### Review of code style/formatting

FreeCAD is in a transitional period: eventually we expect all code to be formatted by automated code styling tools via a pre-commit hook. However, this transition is not yet complete. Until it is, as a best practice developers should work to eliminate any code formatting changes except in code they are actually changing (that is, they should not apply automatic code formatting across an entire file where they are only changing a few lines). Maintainers should err on the side of forgiveness in the event of inadvertent code format changes: if it's straightforward to remove the superfluous changes then it should be done, but a PR should not be rejected for simple formatting issues.

### When in doubt

A major goal during a code review is to provide a good experience for Contributors, and part of that is ensuring consistency across Maintainers. FreeCAD operates predominantly on a consensus basis: if you run into a situation during a code review where you are unsure how to proceed, your first course of action should be to reach out to other Maintainers to see how others have handled such things in the past. Once a course of action has been arrived at, consider documenting the new process here in the Developer's Handbook.

## Onboarding new Contributors

People choose to contribute their time and talents to the FreeCAD project for a variety of reasons: these reasons may or may not align with the strategic goals of the project. When possible, new Contributors should be steered towards projects that address pre-existing Issues, and/or that make progress towards specific Roadmap items. That said, many times a new Contributor develops code to solve a specific problem that they personally encountered when using FreeCAD, and those contributions can still be valuable, even if the Issue is created _after_ the PR.

Without mentorship from others in the community, code contributions from new Contributors can fail to meet community guidelines and expectations. "Onboarding" is the process of guiding new Contributors through the process that we expect all developers to follow. This process is often foreign to those who have not participated in large open-source projects before, and if care isn't taken can seem too onerous or off-putting to be worth their time investment.

There is no one-size-fits-all solution to Maintainer-Contributor relations: different human beings react to these interactions in different ways. What may be "terse and confrontational" to one Contributor is "concise and focused" to another. This difficulty is compounded by language differences in our highly international community of developers. Therefore, this document can only offer the following general guidelines:

1. **Assume good intentions.** The vast majority of code submitted by new developers is intended to solve a specific problem, and represents a time investment into the FreeCAD project that is already above and beyond what most people can contribute. We are right to be grateful for their contribution, even if it needs work before it is ready to merge.
2. **Focus on the infinite goal.** The PR in front of you is inherently short-term: it needs work, and part of a Maintainer's job is to get it into shape. But more importantly, a Maintainer's job is to consider the infinite future horizon of the project as a whole: in any interaction with a new Contributor, mentorship and encouragement _must_ take priority over the short-term goal of "fixing" their code. That said, a Contributor who cannot work constructively with a Maintainer to improve their work is unlikely to be a valuable long-term Contributor, so a balance must be found on a case-by-case basis.
3. **Be polite, courteous, and professional.** Even considering the language and cultural differences we face in our communications, simple use of "please" and "thank you" go a long way towards encouraging civil, professional discourse. *Ad hominem* arguments, character criticism, and other forms of disrespect for the human beings participating in this project have no place here.
4. **Cut your losses.** Your well-being as a Maintainer is valuable too, and you have the right to expect polite, courteous, and professional behavior from those whose code you are reviewing. If you face a situation in which a Contributor refuses to respond or cooperate with your (polite, courteous, and professional) advice, it may be that their contribution does not merit further consideration. You are never obligated to continue a review that you don't feel is proceeding in a constructive way.

### First time contributors require the CI workflow to be started manually

The Github CI/CD pipeline will execute for any PR that is created unless the PR comes from a first-time. This is intentional and allow is meant to catch bad actors attempting to slip malicious code into the CI process.  When a PR requires the workflow to be started manually, take extra time to evaluate the PR and ensure that any change to the CI system is expected and desirable.

## Merge Process

When possible Maintainers should use the web-based PR merge system on GitHub to avoid potential git errors and to maintain process consistency. There are three merge strategies available:
1. Create a merge commit. This has the advantage of preserving the cryptographic commit signature of the individual commits created by the Contributor, if included. It demonstrates the history of the process clearly, and provides links back to the original Pull Request automatically. However, it creates one extra commit per PR, and for very small single-commit PRs may needlessly pollute the project's commit history. Nevertheless, this should be considered the default merge strategy.
2. Rebase and merge. The rebase process changes the commits, so eliminates any cryptographic commit signature that had been provided by the Contributor. It directly includes the commits in the main history of the repository, without any intervening merge commit. This arguably results in a "cleaner" commit history for the project as a whole. This is most appropriate for small, single-commit PRs.
3. Squash and merge. In some cases a Contributor may not be comfortable using git, and may be unable to squash their own commit history into a reasonable number of well-defined commits. In those cases Maintainers have the option of squashing all commits into one during the merge process. This is most appropriate when working with those uncomfortable with using git, who have a needlessly complex commit history for a single simple task.

## Merge Meetings

Every Monday, FreeCAD maintainers meet on a Jitsi call for an hour to go through the list of open pull requests and apply the ones that are ready for merging. The recurring event is [available in the FPA calendar](https://www.freecad.org/events.php).

The call moderator opens the list of all open PRs, then the team of moderators goes through them in reverse chronological order: they look at submitted patches, discuss whether further changes need to happen, and collectively reach decision on merging or postponing the merge.

This event is open for everybody. Developers whose patches are undergoing review are encouraged to join the call in case maintainers have questions about the code / design decisions.

## Reverting

Under almost no circumstances should a Maintainer revert something that they did not themselves merge. If a serious issue is identified with a commit, the committing Maintainer and original Contributor should be notified by either commenting on the original Pull Request, commenting on the commits themselves, or (preferably) by creating a new GitHub Issue and mentioning them in it. It is expected that despite our best efforts, occasionally the master branch will fail to compile on some platforms, that serious unintended feature regressions will occur, etc. None of these things constitute a reason to revert the commit without discussion.

In cases where the committing Maintainer and/or original Contributor cannot be reached in a timely manner, and the bug results in the risk of serious data loss, the commit may be reverted if several Maintainers discuss it and determine that is the best course of action. Under no circumstances should a commit or PR be reverted unilaterally.
