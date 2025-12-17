
# Addon Ecosystem Coordinator

## Unmaintained Addons

To keep the addon ecosystem in a healthy state, it is vital to handle addons that haven't been maintained in a while.

### Intent of Continuation

Generally this involves attempting to contact the author to ask them if they intent to continue maintenance.

Depending on the available contact information this may be done via Email, Discord, GitHub, ..

If the author hasn't responded after a certain period, action might be taken to keep up maintenance.

While waiting on a response to an IoC notice, maintenance steps might already be prepared depending on the likelihood of the author responding.

### Maintenance Steps

There are different followup steps that may be taken depending on the addons state.

Addons that for example rely on services that don't exist anymore are candidate for direct removal as they are effectively non-functional.

Long unmaintained addons that are usually mostly unusable in current versions may be kept for [Safe Keeping](#Safe-Keeping) or removed.

Unmaintained addons that don't fit the previous categories are usually [Adopted](#Adoption).

### Safe Keeping

Addons that once have been functional but are no longer kept up and become incompatible with current versions may be forked for safe keeping.

The addon repository will be forked and possibly given a light cleanup but no support for code changes will be offered.

The repository will effectively act as an archive that ensures it stays as it is.

### Adoption Addons

When adopting an addon, it's repository is forked and given a light cleanup at first.

Depending on the state and widespread usage of the addon, it may be kept mostly as is.

At this point addons usually also receive updates to fixup their licensing, manifest and addons structure.

Afterwards we look for community members that are qualified and willing to take over maintainership.

### Resurrected Authors

Addon authors that had been unresponsive to IoC requests and their addons have been adopted are free to come back and continue working on them again.

