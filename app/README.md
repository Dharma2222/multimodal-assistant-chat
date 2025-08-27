# Git Commands – Quick Guide

Practical Git commands for everyday work.
**Warning:** Commands like `reset --hard` and `clean -fd` are destructive—make sure you truly want to discard local changes/files before running them.

## Table of Contents

* [Check Status](#check-status)
* [Discard All Local Changes](#discard-all-local-changes)
* [Undo Changes in a Specific File](#undo-changes-in-a-specific-file)
* [Remove Untracked Files](#remove-untracked-files)
* [Update Repository (Pull)](#update-repository-pull)
* [Temporarily Save Changes (Stash)](#temporarily-save-changes-stash)
* [Apply Stash](#apply-stash)
* [Manage Multiple Stashes](#manage-multiple-stashes)
* [Apply a Specific Stash](#apply-a-specific-stash)
* [Delete or Pop a Stash](#delete-or-pop-a-stash)
* [Stash Untracked Files](#stash-untracked-files)

---

## Check Status

Show the current state of your working directory and staging area:

```bash
git status
```

## Discard All Local Changes

Discard **all local modifications** and return to the last commit:

```bash
git reset --hard HEAD
```

## Undo Changes in a Specific File

Revert one file back to the last commit:

```bash
git checkout -- <file_name>
# Newer alternative:
git restore --source=HEAD -- <file_name>
```

Example:

```bash
git checkout -- package.json
```

## Remove Untracked Files

Delete **all untracked files/folders**:

```bash
git clean -fd
```

Preview first (safe):

```bash
git clean -fdn
```

## Update Repository (Pull)

Fetch and merge the latest changes from the remote:

```bash
git pull origin <branch_name>
```

Replace `<branch_name>` (e.g., `main`).

## Temporarily Save Changes (Stash)

Save your current uncommitted changes:

```bash
git stash save "<description>"
# Modern equivalent:
git stash push -m "<description>"
```

## Apply Stash

Re-apply the most recent stash (keeps the stash):

```bash
git stash apply
```

## Manage Multiple Stashes

List all saved stashes:

```bash
git stash list
```

## Apply a Specific Stash

Apply a particular stash by its index:

```bash
git stash apply stash@{n}
```

Example:

```bash
git stash apply stash@{2}
```

## Delete or Pop a Stash

Delete a specific stash:

```bash
git stash drop stash@{n}
```

Apply **and** delete in one step:

```bash
git stash pop stash@{n}
```

## Stash Untracked Files

Include untracked files when stashing:

```bash
git stash save -u "<description>"
# or
git stash push -u -m "<description>"
```

---

**Tip:** After applying a stash, you can still revert individual files with `git checkout -- <file>` or `git restore --source=HEAD -- <file>`.
