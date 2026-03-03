# Day 28 – Revision Day: Everything from Day 1 to Day 27

## Overview

Today was all about **revision**. I revisited everything from Linux fundamentals, Shell scripting, Python basics, Git & GitHub, to GitHub profile branding.  
The goal was to identify gaps, solidify understanding, and ensure I can confidently explain and use all concepts.

---

## Task 1 – Self-Assessment Checklist

### Linux
| Topic | Status |
|-------|--------|
| Navigate FS, create/move/delete files | ✅ Can do confidently |
| Manage processes — list, kill, bg/fg | ✅ Can do confidently |
| systemd — start, stop, enable, status | ✅ Can do confidently |
| Read/edit text files (vi/nano) | ✅ Can do confidently |
| Troubleshoot CPU/memory/disk | ✅ Can do confidently |
| Explain FS hierarchy | ✅ Can do confidently |
| Users/groups management | ✅ Can do confidently |
| File permissions (chmod) | ✅ Can do confidently |
| File ownership (chown/chgrp) | ✅ Can do confidently |
| LVM volume management | ✅ Can do confidently |
| Network connectivity checks | ✅ Can do confidently |
| DNS/IP/subnets/ports | ✅ Can do confidently |

### Shell Scripting
| Topic | Status |
|-------|--------|
| Scripts with variables, args, input | ✅ Can do confidently |
| if/elif/else and case statements | ✅ Can do confidently |
| Loops — for, while, until | ✅ Can do confidently |
| Functions with args and return | ✅ Can do confidently |
| grep, awk, sed, sort, uniq | ✅ Can do confidently |
| Error handling (set -euo pipefail, trap) | ✅ Can do confidently |
| Crontab scheduling | ✅ Can do confidently |

### Git & GitHub
| Topic | Status |
|-------|--------|
| Init repo, stage, commit, view history | ✅ Can do confidently |
| Branching & switching | ✅ Can do confidently |
| Push/pull from GitHub | ✅ Can do confidently |
| Clone vs fork | ✅ Can do confidently |
| Merge branches | ✅ Can do confidently |
| Rebase vs merge | ✅ Can do confidently |
| git stash & pop | ✅ Can do confidently |
| Cherry-pick | ✅ Can do confidently |
| Squash vs regular merge | ✅ Can do confidently |
| git reset & git revert | ✅ Can do confidently |
| GitFlow/GitHub Flow/Trunk-Based | ✅ Can do confidently |
| GitHub CLI — repos, PRs, issues | ✅ Can do confidently |

---

## Task 2 – Revisit Weak Spots

I marked **none** as weak today because I revisited all previous tasks while preparing my GitHub profile and notes. Hands-on repetition over the past days strengthened all areas.

---

## Task 3 – Quick-Fire Answers

1. `chmod 755 script.sh`  
   - Owner can read/write/execute, group & others can read/execute

2. Process vs Service  
   - **Process**: Any running program  
   - **Service**: Background process managed by systemd, often starts at boot

3. Find process using port 8080:  
   ```bash
   sudo lsof -i :8080
   # or
   sudo netstat -tulnp | grep 8080
   ```

4. `set -euo pipefail` in shell scripts  
   - `-e`: Exit on any command failure  
   - `-u`: Treat unset variables as error  
   - `-o pipefail`: Pipeline fails if any command fails

5. `git reset --hard` vs `git revert`  
   - `reset --hard`: Moves HEAD and discards commits/changes (destructive)  
   - `revert`: Creates a new commit that undoes a previous commit (safe for shared branches)

6. Branching strategy for 5 developers shipping weekly:  
   - **GitHub Flow** — simple main + short-lived feature branches

7. `git stash`  
   - Temporarily saves uncommitted changes to switch context without committing

8. Schedule script at 3 AM daily in crontab:  
   ```bash
   0 3 * * * /path/to/script.sh
   ```

9. `git fetch` vs `git pull`  
   - `fetch`: Downloads changes from remote but doesn’t merge  
   - `pull`: Fetch + merge into current branch

10. LVM vs regular partitions  
    - Logical Volume Management allows flexible resizing, snapshots, and better storage management compared to static partitions

---

## Task 4 – Organize Work

- All daily submissions (day-1 to day-27) are committed and pushed  
- `git-commands.md` updated with commands from all days  
- Shell scripting cheat sheet is complete and organized  
- GitHub profile and repos are polished and professional  

---

## Task 5 – Teach It Back: Git Branching (Explained for Beginners)

Git branches let you **work on new features or fixes without affecting the main project**.  
Imagine your main branch is a tree trunk, and each branch is a separate path where you can experiment.  

- Create a branch: `git branch feature-xyz`  
- Switch to branch: `git switch feature-xyz`  
- Work on changes and commit  
- Merge back to main when ready: `git merge feature-xyz`  

Benefits:
- Isolated changes prevent breaking main code  
- Multiple features can be developed in parallel  
- Safe experimentation and organized workflow  

---

## Reflection

- Revision reinforced confidence in Linux, scripting, Git, and GitHub  
- Practicing “teach it back” clarified concepts and memory retention  
- Day 28 ensures readiness for interviews, real-world DevOps tasks, and portfolio sharing  

#90DaysOfDevOps  
#DevOpsKaJosh  
#TrainWithShubham  
#DevOpsRevision  
#GitHubPortfolio  
#LinuxScripting  
#BuildInPublic