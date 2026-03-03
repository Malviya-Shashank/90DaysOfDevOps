# Day 24 – Advanced Git: Merge, Rebase, Stash & Cherry Pick

## Overview

Today I practiced advanced Git workflows that are essential in real-world development:

- Merge
- Rebase
- Squash Merge
- Stash
- Cherry Pick

These commands help manage collaboration, maintain clean history, and handle interruptions efficiently.

---

# Task 1 – Git Merge

## Fast-Forward Merge

### Steps Performed

```bash
git checkout main
git checkout -b feature-login

# Make changes
git add .
git commit -m "Add login UI"
git commit -m "Add login validation"

git checkout main
git merge feature-login
```

### Observation

Git performed a **fast-forward merge**.

### What is Fast-Forward Merge?

A fast-forward merge happens when:
- `main` has no new commits.
- Git simply moves the branch pointer forward.
- No merge commit is created.
- History remains linear.

---

## Merge Commit

### Steps Performed

```bash
git checkout -b feature-signup

# Make changes
git add .
git commit -m "Add signup page"

git checkout main
git commit -m "Update README on main"

git merge feature-signup
```

### Observation

Git created a **merge commit** because both branches had new commits.

### When Does Git Create a Merge Commit?

- When both branches have new commits.
- When histories have diverged.
- Git creates a new commit combining both histories.

---

## Merge Conflict

### Creating Conflict Intentionally

1. Edit the same line in a file on `feature-signup`
2. Edit the same line differently on `main`
3. Try merging

```bash
git merge feature-signup
```

Git shows:

```
CONFLICT (content): Merge conflict in file.txt
```

### Resolving Conflict

```bash
# Edit file manually to fix conflict
git add file.txt
git commit -m "Resolve merge conflict"
```

### What is a Merge Conflict?

A merge conflict occurs when Git cannot automatically decide which changes to keep.

---

# Task 2 – Git Rebase

## Steps Performed

```bash
git checkout main
git checkout -b feature-dashboard

git add .
git commit -m "Add dashboard layout"
git commit -m "Add dashboard charts"

git checkout main
git commit -m "Update navbar"

git checkout feature-dashboard
git rebase main
```

---

## What Rebase Does

Rebase:
- Takes commits from feature branch
- Reapplies them on top of latest `main`
- Rewrites commit history
- Creates a linear history

---

## Visualizing History

```bash
git log --oneline --graph --all
```

### Merge History
- Shows branching structure

### Rebase History
- Shows straight linear history

---

## Why You Should Not Rebase Shared Commits

Rebase rewrites commit history.

If commits are already pushed:
- Other developers may have pulled them.
- Rewriting history causes confusion.
- It can break collaboration.

Rule:
Rebase only local, unpublished commits.

---

## When to Use Rebase vs Merge

Use **Rebase**:
- Before creating a PR
- On local branches
- To maintain clean history

Use **Merge**:
- On shared branches
- When preserving full history matters

---

# Task 3 – Squash Merge vs Regular Merge

## Squash Merge

### Steps

```bash
git checkout -b feature-profile

git commit -m "Add profile page"
git commit -m "Fix typo"
git commit -m "Update formatting"
git commit -m "Minor CSS fix"

git checkout main
git merge --squash feature-profile
git commit -m "Add profile feature"
```

### Result

All commits were combined into **one single commit** on `main`.

---

## Regular Merge

```bash
git checkout -b feature-settings

git commit -m "Add settings page"
git commit -m "Add settings validation"

git checkout main
git merge feature-settings
```

### Result

All commits were preserved, and a merge commit was added.

---

## Squash vs Regular Merge

Squash:
- Cleaner history
- Combines small commits
- Loses individual commit history

Regular Merge:
- Preserves complete history
- Shows actual development flow

---

# Task 4 – Git Stash

## Scenario

I made changes without committing.

```bash
# Modify file but do not commit
git checkout another-branch
```

Git prevented switching because changes would be overwritten.

---

## Using Stash

```bash
git stash
```

Switch branch:

```bash
git checkout main
```

Return and restore changes:

```bash
git checkout feature-branch
git stash pop
```

---

## Stash With Message

```bash
git stash push -m "WIP dashboard changes"
```

List stashes:

```bash
git stash list
```

Apply specific stash:

```bash
git stash apply stash@{0}
```

---

## Stash Pop vs Apply

- `git stash pop`
  - Applies changes
  - Removes stash from stash list

- `git stash apply`
  - Applies changes
  - Keeps stash saved

Pop = Apply + Delete  
Apply = Apply only  

---

## Real-World Use Case

- Working on feature
- Urgent bug fix required
- Stash changes
- Fix bug
- Return and pop stash

---

# Task 5 – Cherry Pick

## Steps Performed

```bash
git checkout -b feature-hotfix

git commit -m "Change A"
git commit -m "Critical Bug Fix"
git commit -m "Change C"

git log --oneline
```

Copy the commit hash of **"Critical Bug Fix"**

```bash
git checkout main
git cherry-pick <commit-hash>
```

---

## What Cherry-Pick Does

Cherry-pick:
- Applies a specific commit
- Does not merge entire branch
- Creates a new commit with same changes

---

## When to Use Cherry-Pick

- Production hotfix
- Backport fix to older versions
- Apply specific change only

---

## Risks of Cherry-Picking

- Duplicate commits
- Conflicts may occur
- Messy history if overused

---

# Key Learnings

Today I learned how advanced Git workflows function in real development environments:

- Merge combines histories.
- Rebase rewrites history.
- Squash cleans history.
- Stash handles interruptions.
- Cherry-pick isolates specific changes.

These tools help manage collaboration, maintain clean history, and handle complex workflows confidently.

---

## Tags

#90DaysOfDevOps  
#DevOpsKaJosh  
#TrainWithShubham  
#Git  
#AdvancedGit  
#Merge  
#Rebase  
#Stash  
#CherryPick  
#DevOpsJourney