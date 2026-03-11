# Day 41 – Triggers & Matrix Builds

## Objective

The goal of Day 41 was to learn different ways to **trigger GitHub Actions workflows** and how to run jobs across **multiple environments using Matrix builds**.

Repository used for practice:

https://github.com/shashankmalv/github-actions-practice

---

# Task 1 – Trigger on Pull Request

I created a workflow that runs automatically when a **Pull Request is opened or updated against the main branch**.

Workflow file:

```
.github/workflows/pr-check.yml
```

Branch used for testing:

```
feature-pr-test
```

### pr-check.yml

```yaml
name: PR Check

on:
  pull_request:
    branches:
      - main
    types: [opened, synchronize]

jobs:
  pr-check:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Print PR branch
        run: echo "PR check running for branch: ${{ github.head_ref }}"
```

### Steps Performed

1. Created a new branch

```bash
git checkout -b feature-pr-test
```

2. Added the workflow file

```
.github/workflows/pr-check.yml
```

3. Committed and pushed the branch

```bash
git add .
git commit -m "Add PR check workflow"
git push origin feature-pr-test
```

4. Opened a **Pull Request from `feature-pr-test` → `main`**

### Result

When the Pull Request was opened, GitHub automatically triggered the workflow.

The **PR page displayed the PR Check workflow run**, confirming that the trigger worked correctly.

---

# Task 2 – Scheduled Trigger

I added a **cron schedule trigger** to run a workflow automatically every day.

### Cron Schedule

```yaml
on:
  schedule:
    - cron: '0 0 * * *'
```

### Meaning

```
0 0 * * *
│ │ │ │ │
│ │ │ │ └─ Day of week
│ │ │ └── Month
│ │ └──── Day of month
│ └────── Hour
└──────── Minute
```

This runs the workflow **every day at midnight UTC**.

### Cron Expression Question

Cron expression for **every Monday at 9 AM UTC**:

```
0 9 * * 1
```

---

# Task 3 – Manual Trigger

I created a workflow that can be triggered manually from the **GitHub Actions UI**.

Workflow file:

```
.github/workflows/manual.yml
```

### manual.yml

```yaml
name: Manual Workflow

on:
  workflow_dispatch:
    inputs:
      environment:
        description: "Select environment"
        required: true
        default: "staging"

jobs:
  manual-job:
    runs-on: ubuntu-latest

    steps:
      - name: Print selected environment
        run: echo "The selected environment is ${{ github.event.inputs.environment }}"
```

### Steps Performed

1. Navigated to **GitHub → Actions**
2. Selected **Manual Workflow**
3. Clicked **Run Workflow**
4. Entered environment name (`staging` or `production`)

### Result

The workflow executed and printed the selected environment.

Example output:

```
The selected environment is staging
```

---

# Task 4 – Matrix Builds

Matrix builds allow running the same job across **multiple configurations simultaneously**.

Workflow file:

```
.github/workflows/matrix.yml
```

### matrix.yml

```yaml
name: Matrix Build

on:
  push:

jobs:
  matrix-job:
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest]
        python-version: ["3.10", "3.11", "3.12"]

    runs-on: ${{ matrix.os }}

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}

      - name: Print Python version
        run: python --version
```

### Result

Matrix combinations:

| OS | Python Version |
|----|----|
| Ubuntu | 3.10 |
| Ubuntu | 3.11 |
| Ubuntu | 3.12 |
| Windows | 3.10 |
| Windows | 3.11 |
| Windows | 3.12 |

Total jobs executed:

```
2 OS × 3 Python versions = 6 jobs
```

All jobs ran **in parallel**.

---

# Task 5 – Exclude & Fail-Fast

I modified the matrix configuration to **exclude one combination** and disable fail-fast behavior.

### Updated Matrix Configuration

```yaml
strategy:
  fail-fast: false

  matrix:
    os: [ubuntu-latest, windows-latest]
    python-version: ["3.10", "3.11", "3.12"]

    exclude:
      - os: windows-latest
        python-version: "3.10"
```

### Explanation

Excluded job:

```
Windows + Python 3.10
```

### Fail-Fast Behavior

| Setting | Behavior |
|------|------|
| fail-fast: true (default) | If one job fails, the remaining matrix jobs are cancelled |
| fail-fast: false | Even if one job fails, the remaining jobs continue running |

### Result

All other matrix jobs continued running even when one job failed.

---

# Repository Structure

```
github-actions-practice
│
├── .github
│   └── workflows
│       ├── hello.yml
│       ├── pr-check.yml
│       ├── manual.yml
│       └── matrix.yml
│
└── README.md
```

---

# Practice Repository

All workflows and runs can be found here:

https://github.com/shashankmalv/github-actions-practice

---

# Key Learnings

1. GitHub Actions workflows can be triggered by multiple events such as **push, pull_request, schedule, and manual triggers**.
2. Cron expressions allow **automated scheduled pipelines**.
3. Manual workflows help run pipelines **on-demand**.
4. Matrix builds enable running the same job across **multiple environments simultaneously**.
5. `fail-fast` controls whether other jobs stop when one job fails.

---

# Tags

#90DaysOfDevOps  
#DevOps  
#GitHubActions  
#CICD  
#TrainWithShubham