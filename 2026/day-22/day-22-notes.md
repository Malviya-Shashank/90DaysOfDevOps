# Day 22 – Introduction to Git: My First Repository

## Overview

Today I officially started my Git journey.  
I focused on understanding version control fundamentals and building my first repository from scratch.

This day was about learning Git by doing — not just reading commands.

---

# Task 1 – Install and Configure Git

I verified Git was installed on my system and confirmed the version.  
Then I configured my Git identity (name and email), which Git uses to track commits.

## What I Understood

- Git must be configured before making commits.
- Each commit stores the author's name and email.
- Global configuration applies to all repositories on the system.
- Git keeps track of changes using this identity information.

## Why It Is Important

In real-world teams, Git history shows who made which change.  
Proper configuration ensures accountability and clean collaboration.

---

# Task 2 – Creating My First Repository

I created a new project folder and initialized it as a Git repository.

After running `git init`, I explored the hidden `.git/` directory.

## What I Understood

- `git init` converts a normal folder into a Git repository.
- The `.git` directory stores all commit history and repository metadata.
- Git tracks changes from that moment onward.
- If the `.git` folder is deleted, the entire version history is lost.

## Why It Is Important

Understanding `.git` helps me realize that Git is not magic —  
it is a structured system that stores objects, references, and commit history internally.

This knowledge is useful for troubleshooting and deeper Git learning later.

---

# Task 3 – Creating My Git Commands Reference

I created a file called `git-commands.md` to document all Git commands I used.

I organized them into categories like:
- Setup & Configuration
- Basic Workflow
- Viewing Changes

For each command, I wrote:
- What it does
- An example usage

## What I Understood

- Writing documentation improves understanding.
- Organizing commands makes learning structured.
- Git learning is continuous — this file will grow daily.

## Why It Is Important

This file becomes my personal Git handbook.  
As I learn more advanced concepts (branches, merges, rebasing), I will update it.

---

# Task 4 – Staging and Committing

I practiced staging files and committing them with meaningful messages.

I checked:
- What is in the working directory
- What is staged
- What is committed

Then I viewed my commit history.

## What I Understood

- `git add` moves changes to the staging area.
- `git commit` permanently records staged changes.
- Clear commit messages are very important.
- `git log` shows commit history with details like author, date, and message.

## Why It Is Important

The staging area allows selective commits instead of committing everything at once.

This helps:
- Keep commit history clean
- Group related changes together
- Make debugging easier later

---

# Task 5 – Building Commit History

I made multiple edits to my files and committed them separately to build a proper history.

I practiced:
- Checking what changed since the last commit
- Staging selectively
- Writing descriptive commit messages
- Viewing history in compact format

## What I Understood

- Small, frequent commits are better than large commits.
- Each commit is like a snapshot of the project.
- Git history helps track how the project evolved.

## Why It Is Important

In DevOps and production environments:
- Clean commit history helps trace issues.
- You can identify when a bug was introduced.
- Teams can collaborate more efficiently.

---

# Understanding the Git Workflow

## 1️⃣ Difference Between `git add` and `git commit`

- `git add` prepares changes for commit.
- `git commit` permanently records those prepared changes.

Add = Preparation  
Commit = Final Save

---

## 2️⃣ What Does the Staging Area Do?

The staging area acts as a checkpoint between the working directory and the repository.

It allows you to:
- Select specific changes
- Group related updates
- Avoid committing unwanted files

Git does not commit directly because developers need control over what gets saved.

---

## 3️⃣ What Does `git log` Show?

`git log` shows:
- Commit hash
- Author name
- Date and time
- Commit message

It provides the full history of the repository.

---

## 4️⃣ What is the `.git` Folder?

The `.git` folder:
- Stores all commits
- Contains repository metadata
- Manages branches and references

If deleted, the entire version history is permanently lost.

---

## 5️⃣ Working Directory vs Staging Area vs Repository

- **Working Directory** → Where files are edited.
- **Staging Area** → Where changes are prepared.
- **Repository** → Where committed changes are permanently stored.

Understanding this flow is the foundation of Git.

---

# Key Takeaways

- Git is a distributed version control system.
- The staging area provides granular control over commits.
- Commit history should be clean and meaningful.
- The `.git` folder is the heart of the repository.
- Version control is essential for DevOps workflows.

---

# My Reflection

Today I moved beyond just running Git commands —  
I started understanding how Git actually works.

This foundation will help me with:
- Infrastructure as Code
- CI/CD pipelines
- Collaborative development
- Production deployments

Git is not just a tool — it is the backbone of modern DevOps.

---

## Tags

#90DaysOfDevOps  
#DevOpsKaJosh  
#TrainWithShubham  
#Git  
#VersionControl  
#DevOpsJourney  
#OpenSource