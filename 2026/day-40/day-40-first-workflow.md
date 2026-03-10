# Day 40 – Your First GitHub Actions Workflow

## Objective

The goal of Day 40 was to create my **first CI workflow using GitHub Actions** and understand how automation pipelines work in real DevOps environments.

This was the first time running a **CI pipeline in the cloud** triggered automatically by a GitHub push.

Repository used for practice:

https://github.com/shashankmalv/github-actions-practice

---

# Task 1 – Repository Setup

I created a new public repository for practicing GitHub Actions.

Repository:

https://github.com/shashankmalv/github-actions-practice

### Steps Performed

1. Created repository `github-actions-practice`
2. Cloned the repository locally

```bash
git clone https://github.com/shashankmalv/github-actions-practice.git
cd github-actions-practice
```

3. Created the GitHub Actions workflow directory

```bash
mkdir -p .github/workflows
```

---

# Task 2 – Hello Workflow

I created my first GitHub Actions workflow file.

File location:

```
.github/workflows/hello.yml
```

### hello.yml

```yaml
name: Hello Workflow

on:
  push:

jobs:
  greet:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Print greeting
        run: echo "Hello from GitHub Actions!"
```

### Result

After pushing the workflow file, GitHub automatically triggered the pipeline.

Pipeline status was **successful (green)**.

---

# Task 3 – Workflow Anatomy

I studied the key components of the workflow file.

| Key | Description |
|----|----|
| `on:` | Defines the event that triggers the workflow (e.g., push, pull request) |
| `jobs:` | Contains all jobs that will run in the workflow |
| `runs-on:` | Specifies the runner environment (e.g., ubuntu-latest) |
| `steps:` | Sequence of tasks executed in a job |
| `uses:` | Runs a predefined GitHub Action |
| `run:` | Executes shell commands on the runner |
| `name:` | Optional label used to identify a step |

---

# Task 4 – Extended Workflow

I updated the workflow to include additional steps.

Updated `hello.yml`:

```yaml
name: Hello Workflow

on:
  push:

jobs:
  greet:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Print greeting
        run: echo "Hello from GitHub Actions!"

      - name: Show current date and time
        run: date

      - name: Print branch name
        run: echo "Branch name is ${{ github.ref_name }}"

      - name: List repository files
        run: ls -la

      - name: Show runner OS
        run: uname -a
```

### What this workflow now does

- Prints a greeting message
- Displays current date and time
- Shows the branch that triggered the workflow
- Lists repository files
- Displays the operating system of the GitHub runner

---

# Task 5 – Pipeline Failure Test

To understand pipeline errors, I intentionally added a failing step.

Example:

```yaml
- name: Intentional failure
  run: exit 1
```

### Observation

The pipeline failed and GitHub marked the workflow with a **red ❌ status**.

By opening the failed job in the **Actions tab**, I was able to see:

- The failing step
- Error logs
- The exact command that caused the failure

After removing the failing command, the pipeline ran successfully again.

---

# Key Learnings

1. GitHub Actions automatically runs workflows when triggered by events like `push`.
2. Workflow files must be stored inside `.github/workflows/`.
3. Jobs run on GitHub-hosted runners such as `ubuntu-latest`.
4. Each workflow is composed of **jobs → steps → commands**.
5. Pipeline failures help detect issues early in the development lifecycle.

---

# Repository Structure

```
github-actions-practice
│
├── .github
│   └── workflows
│       └── hello.yml
│
└── README.md
```

---

# Practice Repository

All workflow experiments and runs can be found in the repository:

https://github.com/shashankmalv/github-actions-practice

---

# Tags

#90DaysOfDevOps  
#DevOps  
#GitHubActions  
#CICD  
#TrainWithShubham