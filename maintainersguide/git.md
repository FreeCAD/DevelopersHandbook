---
layout: default
---

# Git and Github

## Checking out PRs locally for review

Git can [checkout pull requests from a remote repository](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/checking-out-pull-requests-locally). This makes it easier to review pull requests locally, and to run tests against them.

## Configuring Git to make checkout easier.

You can modify your .gitconfig file to include the PR namespace.  This allows easier checkout of PRs and also
shows the PRs in graphical tools like gitk.

### edit the .git/config file

Make sure that you have a remote defined in your .git/config file that corresponds to the main FreeCAD repo.
Ideally, this remote is named "upstream".  If you don't have this remote defined, you can add it like this:

```bash
[remote "upstream"]
	url = git@github.com:FreeCAD/FreeCAD.git
	fetch = +refs/heads/*:refs/remotes/upstream/*
```

### add the PR namespace
add a second fetch line to the upstream remote definition.  This line will fetch the PR namespace from the upstream repo.
```
    fetch = +refs/pull/*/head:refs/remotes/upstream/pr/*
```

### now you can fetch PRs with the command 
```bash
git fetch upstream
```

### checkout a PR
```bash
git checkout upstream/pr/1234
```




