---
title: Maintainers Guide
description:
  Guidelines for Maintainers regarding code review and merge procedures.
layout: default
---

# {{page.title}}

{{page.description}}

## The role of a Maintainer

Maintainers have two primary responsibilities:
 - To participate in Issue discussions to help guide the resolution of that Issue, including refining, splitting, closing duplicates, and ensuring that all discussion participants are professional, inclusive, and working towards a common goal of improving FreeCAD.
 - Once an Issue has been identified and agreed upon and a PR has been submitted to resolve it, Maintainers review the code itself to identify major defects and conflicts with FreeCAD's coding guidelines. If the PR is by a new Contributor, Maintainers work to onboard that Contributor, ensuring they feel welcome, and recieve the mentorship they need to succeed in this project.

## CONTRIBUTING.md

The FreeCAD contribution process is governed by the [CONTRIBUTING.md](https://github.com/FreeCAD/FreeCAD/blob/master/CONTRIBUTING.md) document. In particular, that document describes the expected process to go from an Issue to a merged piece of code. All Maintainers should be familiar with this document, and work to follow the process that it outlines. In particular, once an Issue has an agreed-upon solution path, then a PR that conforms to that path should be merged without further debate about the merits of the solution: code review focuses solely on whether the PR does indeed follow the agreed-upon path, and on identifying major defects in the code. Minor defects may be included in the review process, but should not prevent the PR from being merged in a timely manner.

## Code Review Process

Anyone who wishes to participate in the code review process may do so, but all participants should strive to uphold the tenets of CONTRIBUTING.md -- Maintainers are responsible for guiding the review process, and should assist in keeping the discussions on-task. Important qualities to evaluate during a code review are:
1. Does the coded solution conform to the agreed-upon path in the Issue that the PR addresses?
2. Does the code compile on all platforms?
3. Does the code pass the CI test suite?
4. Does the code include new tests to validate its functionality? Note that we do not have a firm requirement that tests be provided, but we should strive to do so when possible.
5. Is the code written in a reasonably easy-to-understand manner, following modern best-practices for the language it is written in? Not all "best practices" violations represent major defects, but care should be taken to guide Contributors toward submitting the best code they can. Merging code with minor best-practices defects and advising Contributors to address those defects in follow-on PRs the encouraged workflow.
6. Are the commits well-structured, with clear commit messages following the guidelines outlined in CONTRIBUTING.md?
7. Are there any copyright issues that must be addressed? (e.g. use of logos or graphics from other organizations, code copied from other projects, etc.) Any copyrighted content (logos, graphics, code, etc.) included in FreeCAD must have a compatible license, and should have clear permission for us to use it. Of course, it must also be used in a way that's beneficial to FreeCAD's users. 

## Onboarding new Contributors

People choose to contribute their time and talents to the FreeCAD project for a variety of reasons: these reasons may or may not align with the strategic goals of the project. When possible, new Contributors should be steered towards projects that address pre-existing Issues, and/or that make progress towards specific Roadmap items. That said, many times a new Contributor develops code to solve a specific problem that they personally encountered when using FreeCAD, and those contributions can still be valuable, even if the Issue is created _after_ the PR.

Without mentorship from others in the community, code contributions from new Contributors can fail to meet community guidelines and expectations. "Onboarding" is the process of guiding new Contributors through the process that we expect all developers to follow. This process is often foreign to those who have not participated in large open-source projects before, and if care isn't taken can seem too onerous or off-putting to be worth their time investment.

There is no one-size-fits-all solution to Maintainer-Contributor relations: different human beings react to these interactions in different ways. What may be "terse and confrontational" to one Contributor is "consise and focused" to another. This difficulty is compounded by language differences in our highly international community of developers. Therefore, this document can only offer the following general guidelines:

1. **Assume good intentions.** The vast majority of code submitted by new developers is intended to solve a specific problem, and represents a time investment into the FreeCAD project that is already above and beyond what most people can contribute. We are right to be grateful for their contribution, even if it needs work before it is ready to merge.
2. **Focus on the infinite goal.** The PR in front of you is inherently short-term: it needs work, and part of a Maintainer's job is to get it into shape. But more importantly, a Maintainer's job is to consider the infinite future horizon of the project as a whole: in any interaction with a new Contributor, mentorship and encouragement _must_ take priority over the short-term goal of "fixing" their code. That said, a Contributor who cannot work constructively with a Maintainer to improve their work is unlikely to be a valuable long-term Contributor, so a balance must be found on a case-by-case basis.
3. **Be polite, courteous, and professional.** Even considering the language and cultural differences we face in our communications, simple use of "please" and "thank you" go a long way towards encouraging civil, professional discourse. *Ad hominem* arguments, character criticism, and other forms of disrespect for the human beings participating in this project have no place here.
4. **Cut your losses.** Your well-being as a Maintainer is valuable too, and you have the right to expect polite, courteous, and professional behavior from those whose code you are reviewing. If you face a situation in which a Contributor refuses to respond or cooperate with your (polite, courteous, and professional) advice, it may be that their contribution does not merit further consideration. You are never obligated to continue a review that you don't feel is proceeding in a constructive way.

## Merge Process

When possible Maintainers should use the web-based PR merge system on GitHub to avoid potential git errors and to maintain process consistency. There are three merge strategies available:
1. Create a merge commit. This has the advantage of preserving the cryptographic commit signature of the individual commits created by the Contributor, if included. It demonstrates the history of the process clearly, and provides links back to the original Pull Request automatically. However, it creates one extra commit per PR, and for very small single-commit PRs may needlessly pollute the project's commit history. Nevertheless, this should be considered the default merge strategy.
2. Rebase and merge. The rebase process changes the commits, so eliminates any cryptographic commit signature that had been provided by the Contributor. It directly includes the commits in the main history of the repository, without any intervening merge commit. This arguably results in a "cleaner" commit history for the project as a whole. This is most appropriate for small, single-commit PRs.
3. Squash and merge. In some cases a Contributor may not be comfortable using git, and may be unable to squash their own commit history into a reasonable number of well-defined commits. In those cases Maintainers have the option of squashing all commits into one during the merge process. This is most appropriate when working with those uncomfortable with using git, who have a needlessly complex commit history for a single simple task.
