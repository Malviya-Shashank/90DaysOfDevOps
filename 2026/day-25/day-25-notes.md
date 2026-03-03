# Day 25 – Git Reset vs Revert & Branching Strategies

## Overview

Today I learned how to safely undo mistakes in Git using:

- `git reset`
- `git revert`

I also explored real-world branching strategies used by engineering teams at scale.

Understanding when to rewrite history vs when to preserve it is critical in collaborative environments.

---

# Task 1 – Git Reset

## Step 1: Create 3 Commits

```bash
git commit -m "Commit A"
git commit -m "Commit B"
git commit -m "Commit C"

git log --oneline
```

---

## 1️⃣ git reset --soft

```bash
git reset --soft HEAD~1
```

### What Happened?

- Commit C was removed.
- Changes from commit C stayed in the staging area.
- Files were still ready to commit.

### Use Case

- When you want to rewrite the last commit message.
- When you want to combine commits.

---

## 2️⃣ git reset --mixed (Default)

Re-commit C, then:

```bash
git reset --mixed HEAD~1
```

### What Happened?

- Commit C was removed.
- Changes moved to working directory.
- Files were NOT staged.

### Use Case

- When you want to modify changes before committing again.

---

## 3️⃣ git reset --hard

Re-commit C, then:

```bash
git reset --hard HEAD~1
```

### What Happened?

- Commit C was deleted.
- Changes were permanently removed from working directory.
- No trace in working tree.

⚠️ This is destructive.

---

## Difference Between --soft, --mixed, --hard

| Mode | Removes Commit | Keeps Changes Staged | Keeps Changes Unstaged | Deletes Changes |
|------|----------------|---------------------|------------------------|-----------------|
| --soft | ✅ | ✅ | ❌ | ❌ |
| --mixed | ✅ | ❌ | ✅ | ❌ |
| --hard | ✅ | ❌ | ❌ | ✅ |

---

## Which One Is Destructive?

`--hard` is destructive because it permanently deletes uncommitted changes.

---

## Should You Use Reset on Pushed Commits?

No.

If commits are already pushed:
- Reset rewrites history.
- Other developers may have based work on those commits.
- It causes major collaboration issues.

Rule:
Use `git reset` only on local commits.

---

# Task 2 – Git Revert

## Step 1: Create 3 Commits

```bash
git commit -m "Commit X"
git commit -m "Commit Y"
git commit -m "Commit Z"

git log --oneline
```

---

## Step 2: Revert Middle Commit

```bash
git revert <commit-hash-of-Y>
```

### What Happened?

- A new commit was created.
- It reversed changes introduced by Commit Y.
- Commit Y is still visible in history.

---

## Is Commit Y Still in History?

Yes.

`git revert` does NOT remove commits.
It adds a new commit that undoes changes.

---

## Reset vs Revert

### How is Revert Different?

- `git reset` rewrites history.
- `git revert` preserves history.

---

## Why is Revert Safer?

Because:

- It does not delete commits.
- It works safely on shared branches.
- It maintains full project history.

---

## When to Use Revert vs Reset

Use **Reset**:
- For local mistakes.
- Before pushing commits.

Use **Revert**:
- For production fixes.
- On shared branches.
- When history must remain intact.

---

# Task 3 – Reset vs Revert Comparison

| | `git reset` | `git revert` |
|---|---|---|
| What it does | Moves branch pointer backward | Creates new commit that undoes changes |
| Removes commit from history? | ✅ Yes | ❌ No |
| Safe for shared branches? | ❌ No | ✅ Yes |
| Rewrites history? | ✅ Yes | ❌ No |
| When to use | Local cleanup | Undo changes safely in shared repos |

---

# Task 4 – Branching Strategies

---

## 1️⃣ GitFlow

### How It Works

- `main` → production
- `develop` → integration branch
- `feature/*` → new features
- `release/*` → release preparation
- `hotfix/*` → urgent production fixes

### Diagram

```
main ────────────────●──────────
        \           /
develop ──●──●──●──●──●──●──
          \        /
        feature  release
```

### Used In

- Large enterprise projects
- Scheduled releases

### Pros

- Structured workflow
- Clear separation of environments
- Good for versioned releases

### Cons

- Complex
- Too heavy for small teams

---

## 2️⃣ GitHub Flow

### How It Works

- Single `main` branch
- Create feature branch
- Open Pull Request
- Review & Merge

### Diagram

```
main ───●────●────●────●
        \        /
      feature  feature
```

### Used In

- SaaS companies
- Continuous deployment teams

### Pros

- Simple
- Fast
- Easy collaboration

### Cons

- Less structured for large release cycles

---

## 3️⃣ Trunk-Based Development

### How It Works

- Everyone commits to `main`
- Short-lived branches (1–2 days max)
- Frequent integration

### Diagram

```
main ─●─●─●─●─●─●─●─●
      |  |  |  |
     fix feat fix feat
```

### Used In

- High-performance engineering teams
- CI/CD focused teams
- Companies practicing continuous delivery

### Pros

- Very fast integration
- Fewer merge conflicts
- Encourages small commits

### Cons

- Requires strong CI/CD
- Risky without good testing

---

## Strategy Selection

### Startup Shipping Fast?

GitHub Flow or Trunk-Based Development  
(Simple, fast, minimal overhead)

---

### Large Team with Scheduled Releases?

GitFlow  
(Structured, stable, version-based releases)

---

### Open Source Example

Many open-source projects like Kubernetes use a variation of GitHub Flow with protected branches and PR-based workflow.

---

# git reflog – Safety Net

If something goes wrong:

```bash
git reflog
```

Reflog shows:
- All recent HEAD movements
- Even after hard reset
- Allows recovery of lost commits

---

# Key Learnings

- `git reset` rewrites history.
- `git revert` preserves history.
- Never reset pushed commits.
- Revert is safest for shared branches.
- Branching strategy depends on team size and release model.
- GitFlow is structured.
- GitHub Flow is simple.
- Trunk-Based Development is fast and CI-driven.

Understanding undo strategies is critical to avoid production disasters.

---

## Tags

#90DaysOfDevOps  
#DevOpsKaJosh  
#TrainWithShubham  
#Git  
#GitReset  
#GitRevert  
#BranchingStrategy  
#GitFlow  
#GitHubFlow  
#TrunkBasedDevelopment  
#DevOpsJourney