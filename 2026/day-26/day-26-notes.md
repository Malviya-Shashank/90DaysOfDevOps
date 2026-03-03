# Day 26 – GitHub CLI: Manage GitHub from Your Terminal

## Overview

Today I learned how to use the **GitHub CLI (`gh`)** to manage GitHub directly from the terminal.

Instead of switching to the browser to:
- Create repositories
- Open pull requests
- Manage issues
- Check workflow runs

I can now do everything from the command line — which is powerful for DevOps automation and scripting.

---

# Task 1 – Install and Authenticate

## Install GitHub CLI

### macOS
```bash
brew install gh
```

### Ubuntu/Debian
```bash
sudo apt install gh
```

### Windows (PowerShell)
```powershell
winget install --id GitHub.cli
```

---

## Authenticate

```bash
gh auth login
```

Follow prompts:
- Choose GitHub.com
- HTTPS
- Login via browser

---

## Verify Login

```bash
gh auth status
```

This shows:
- Logged-in account
- Active host
- Authentication status

---

## Authentication Methods Supported by `gh`

- Browser-based OAuth login
- Personal Access Token (PAT)
- SSH authentication
- GitHub Enterprise authentication

---

# Task 2 – Working with Repositories

## Create a New Repository

```bash
gh repo create test-gh-cli-repo --public --readme
```

This:
- Creates repo on GitHub
- Initializes with README
- Makes it public

---

## Clone Using `gh`

```bash
gh repo clone username/test-gh-cli-repo
```

Alternative:
```bash
gh repo clone owner/repo-name
```

---

## View Repository Details

```bash
gh repo view username/test-gh-cli-repo
```

---

## List All Your Repositories

```bash
gh repo list
```

Limit results:

```bash
gh repo list --limit 20
```

---

## Open Repo in Browser

```bash
gh repo view --web
```

---

## Delete Repository

⚠️ Be careful!

```bash
gh repo delete username/test-gh-cli-repo --confirm
```

---

# Task 3 – Issues

## Create an Issue

```bash
gh issue create \
  --title "Bug: Login not working" \
  --body "Login fails when password contains special characters." \
  --label "bug"
```

---

## List Open Issues

```bash
gh issue list
```

---

## View Specific Issue

```bash
gh issue view 1
```

---

## Close an Issue

```bash
gh issue close 1
```

---

## Using `gh issue` in Automation

Possible automation use cases:

- Automatically create issues from monitoring alerts
- Generate issues from CI failures
- Auto-close issues after deployment
- Script-based issue reporting

Example in script:

```bash
gh issue create --title "Nightly Build Failed" --body "Check CI logs."
```

---

# Task 4 – Pull Requests

## Create Branch & Push

```bash
git checkout -b feature-cli-test
echo "Test change" >> test.txt
git add .
git commit -m "Add CLI test file"
git push -u origin feature-cli-test
```

---

## Create Pull Request

```bash
gh pr create --fill
```

Or manually:

```bash
gh pr create \
  --title "Add CLI test file" \
  --body "This PR adds a test file for GitHub CLI practice."
```

---

## List Open PRs

```bash
gh pr list
```

---

## View PR Details

```bash
gh pr view 1
```

Include checks & status:

```bash
gh pr view 1 --json state,reviewDecision,statusCheckRollup
```

---

## Merge PR from Terminal

```bash
gh pr merge 1
```

---

## Merge Methods Supported

`gh pr merge` supports:

- `--merge` (default merge commit)
- `--squash`
- `--rebase`

Example:

```bash
gh pr merge 1 --squash
```

---

## Reviewing Someone Else’s PR

```bash
gh pr checkout 2
```

Add review:

```bash
gh pr review 2 --approve
```

Or request changes:

```bash
gh pr review 2 --request-changes --body "Fix validation logic."
```

---

# Task 5 – GitHub Actions & Workflow Runs

## List Workflow Runs

```bash
gh run list
```

Target specific repo:

```bash
gh run list --repo owner/repo
```

---

## View Workflow Run Details

```bash
gh run view <run-id>
```

---

## How `gh run` and `gh workflow` Help in CI/CD

They allow:

- Monitoring CI status from terminal
- Triggering workflows manually
- Downloading logs for debugging
- Automating deployment checks
- Integrating GitHub Actions with shell scripts

Example:

```bash
gh workflow list
gh workflow run "CI Pipeline"
```

This is powerful for DevOps automation.

---

# Task 6 – Useful `gh` Tricks

## 1️⃣ Raw GitHub API Calls

```bash
gh api repos/owner/repo
```

Useful for automation and scripting.

---

## 2️⃣ GitHub Gists

Create gist:

```bash
gh gist create file.txt --public
```

List gists:

```bash
gh gist list
```

---

## 3️⃣ Releases

Create release:

```bash
gh release create v1.0.0 --title "Version 1.0.0" --notes "Initial release"
```

---

## 4️⃣ Create Aliases

```bash
gh alias set prs "pr list"
```

Now you can run:

```bash
gh prs
```

---

## 5️⃣ Search Repositories

```bash
gh search repos devops --limit 5
```

---

# Key Learnings

Today I learned:

- GitHub can be fully managed from the terminal.
- `gh` integrates Git, GitHub, and CI workflows.
- Pull requests can be created and merged without browser.
- Issues and releases can be automated.
- GitHub Actions can be monitored via CLI.
- `gh` is powerful for DevOps scripting and automation.

This completes my Git & GitHub workflow mastery from Days 22–26.

---

## Tags

#90DaysOfDevOps  
#DevOpsKaJosh  
#TrainWithShubham  
#GitHubCLI  
#gh  
#DevOps  
#Automation  
#CI_CD  
#GitHub  
#DevOpsJourney