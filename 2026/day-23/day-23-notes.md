# Day 23 – Git Branching & Working with GitHub

## Overview

Today I explored one of the most powerful concepts in Git — **branching**.  
I also connected my local repository to GitHub and worked with remotes for the first time.

This day helped me understand how real-world teams collaborate without breaking production code.

---

# Task 1 – Understanding Branches

## 1️⃣ What is a branch in Git?

A branch is an independent line of development.  
It allows you to work on features, fixes, or experiments without affecting the main codebase.

By default, Git creates a `main` branch.

---

## 2️⃣ Why use branches instead of committing everything to main?

If we commit everything directly to `main`:

- The production branch becomes unstable.
- Bugs can break working code.
- Multiple developers may conflict with each other.

Branches allow safe, isolated development.

---

## 3️⃣ What is HEAD in Git?

`HEAD` is a pointer that refers to the current branch or commit you are working on.

When you switch branches, `HEAD` moves to that branch.

---

## 4️⃣ What happens to files when switching branches?

When switching branches:

- Git updates your working directory to match the selected branch.
- Files that exist only in one branch may disappear when switching.
- Commits unique to a branch are not visible in other branches unless merged.

This shows that each branch maintains its own history.

---

# Task 2 – Branching Hands-On Practice

In my `devops-git-practice` repository, I:

- Listed all branches.
- Created a new branch called `feature-1`.
- Switched to `feature-1`.
- Created and switched to `feature-2` in a single command.
- Used `git switch` to move between branches.
- Made a commit on `feature-1` that did not exist on `main`.
- Switched back to `main` and verified that the commit was not there.
- Deleted an unnecessary branch.

---

## What I Understood

- A new branch starts from the current commit.
- Changes made in one branch do not affect another unless merged.
- `git switch` is a cleaner, modern alternative to `git checkout` for branch switching.
- Deleting unused branches keeps the repository clean.

---

## git switch vs git checkout

- `git checkout` is multifunctional (switch branches, restore files).
- `git switch` is focused only on switching branches.
- `git switch` reduces accidental mistakes.

Modern Git recommends using `git switch` for branch changes.

---

# Task 3 – Pushing to GitHub

I created a new repository on GitHub and connected my local repository to it.

Then I:

- Added a remote repository.
- Pushed the `main` branch.
- Pushed the `feature-1` branch.
- Verified both branches were visible on GitHub.

---

## What I Understood

- GitHub acts as a remote repository.
- `origin` is the default name for the remote repository.
- Pushing uploads local commits to GitHub.
- Each branch must be pushed separately the first time.

---

## Difference Between origin and upstream

- **origin** → Your default remote repository (usually your fork or main remote).
- **upstream** → The original repository you forked from.

Origin is where you push your changes.  
Upstream is where you pull updates from if you forked a project.

---

# Task 4 – Pulling Changes from GitHub

I made a change directly on GitHub using the web editor.  
Then I pulled that change into my local repository.

---

## git fetch vs git pull

- `git fetch` downloads changes but does not merge them.
- `git pull` downloads and automatically merges changes into your current branch.

Fetch = Safe preview  
Pull = Fetch + Merge

Understanding this difference is important to avoid unexpected merges.

---

# Task 5 – Clone vs Fork

I practiced both cloning and forking a public repository.

---

## What is Clone?

Cloning creates a local copy of a remote repository.

It is used when:
- You want to work locally on a project.
- You already have write access to the repository.

---

## What is Fork?

Forking creates a copy of someone else's repository under your GitHub account.

It is used when:
- You want to contribute to an open-source project.
- You do not have direct write access to the original repository.

Fork is a GitHub feature, not a Git feature.

---

## How to Keep a Fork Updated?

To sync a fork:

1. Add the original repository as `upstream`.
2. Fetch changes from upstream.
3. Merge or rebase those changes into your branch.

This ensures your fork stays updated with the original project.

---

# Key Concepts Learned Today

- Branches allow isolated development.
- `HEAD` points to the current working branch.
- Commits are branch-specific until merged.
- `origin` and `upstream` serve different remote purposes.
- `git fetch` and `git pull` behave differently.
- Clone copies a repo locally.
- Fork creates a personal copy on GitHub.

---

# My Reflection

Today I moved from basic Git usage to collaborative Git workflows.

Branching made me understand how real-world teams:
- Build features safely
- Avoid breaking production
- Work in parallel
- Manage code across environments

Understanding GitHub integration is a major step toward real DevOps workflows.

This was a big upgrade from just committing locally to working with distributed version control.

---

## Tags

#90DaysOfDevOps  
#DevOpsKaJosh  
#TrainWithShubham  
#Git  
#GitHub  
#Branching  
#VersionControl  
#DevOpsJourney