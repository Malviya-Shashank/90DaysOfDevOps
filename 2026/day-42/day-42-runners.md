# Day 42 – Runners: GitHub-Hosted & Self-Hosted

## Objective

The goal of Day 42 was to understand how **GitHub Actions runners work**, including both **GitHub-hosted runners** and **self-hosted runners**.

Every GitHub Actions job runs on a machine called a **runner**. GitHub provides managed runners, but it is also possible to register and use your own machine as a runner.

Repository used for practice:

https://github.com/shashankmalv/github-actions-practice

---

# Task 1 – GitHub-Hosted Runners

I created a workflow with **three jobs running on different operating systems**.

Workflow file:

```
.github/workflows/hosted-runners.yml
```

### hosted-runners.yml

```yaml
name: Hosted Runners Demo

on:
  push:

jobs:

  ubuntu-job:
    runs-on: ubuntu-latest

    steps:
      - name: Print OS info
        run: |
          echo "Operating System:"
          uname -a
          echo "Hostname:"
          hostname
          echo "User:"
          whoami

  windows-job:
    runs-on: windows-latest

    steps:
      - name: Print OS info
        run: |
          echo OS Information
          hostname

  macos-job:
    runs-on: macos-latest

    steps:
      - name: Print OS info
        run: |
          echo "Operating System:"
          uname -a
          echo "Hostname:"
          hostname
          echo "User:"
          whoami
```

### Result

All three jobs ran **in parallel on different operating systems**:

- Ubuntu
- Windows
- macOS

### Notes

A **GitHub-hosted runner** is a virtual machine provided and managed by GitHub to run workflow jobs.

GitHub automatically:

- Creates the VM
- Installs common development tools
- Runs the workflow
- Deletes the VM after completion

---

# Task 2 – Explore Pre-installed Tools

I checked which tools are already installed on the **ubuntu-latest runner**.

### Step Added

```yaml
- name: Check installed tools
  run: |
    docker --version
    python --version
    node --version
    git --version
```

### Example Output

```
Docker version 24.x
Python 3.x
Node v20.x
git version 2.x
```

### Notes

GitHub-hosted runners come with many **pre-installed tools**, such as:

- Docker
- Python
- Node.js
- Java
- Git
- Build tools

This is useful because it **reduces setup time in CI pipelines**.

---

# Task 3 – Set Up a Self-Hosted Runner

I registered my own machine as a **self-hosted runner**.

### Steps Performed

1. Opened repository settings:

```
GitHub Repo → Settings → Actions → Runners
```

2. Clicked **New Self-Hosted Runner**

3. Selected **Linux**

4. GitHub generated commands to configure the runner.

### Commands Used

```bash
mkdir actions-runner && cd actions-runner

curl -o actions-runner-linux-x64.tar.gz -L https://github.com/actions/runner/releases/latest/download/actions-runner-linux-x64.tar.gz

tar xzf actions-runner-linux-x64.tar.gz

./config.sh --url https://github.com/shashankmalv/github-actions-practice --token <TOKEN>

./run.sh
```

### Result

The runner appeared in GitHub as:

```
Status: Idle
```

with a **green dot** indicating it is online.

---

# Task 4 – Use Self-Hosted Runner

I created a workflow that runs jobs on my **self-hosted runner**.

Workflow file:

```
.github/workflows/self-hosted.yml
```

### self-hosted.yml

```yaml
name: Self Hosted Runner Test

on:
  push:

jobs:
  self-hosted-job:
    runs-on: self-hosted

    steps:
      - name: Print hostname
        run: hostname

      - name: Print working directory
        run: pwd

      - name: Create test file
        run: echo "Hello from self-hosted runner" > test-file.txt

      - name: Verify file
        run: ls -l
```

### Result

The workflow ran on **my own machine instead of GitHub infrastructure**.

After the run completed, the file:

```
test-file.txt
```

was visible on my machine.

---

# Task 5 – Runner Labels

I added a custom label to my runner:

```
my-linux-runner
```

Then updated the workflow:

```yaml
runs-on: [self-hosted, my-linux-runner]
```

### Result

The workflow still executed successfully using the **labeled runner**.

### Notes

Labels are useful when you have **multiple self-hosted runners**, for example:

- GPU runner
- Linux runner
- High-memory runner

This allows workflows to **target specific machines**.

---

# GitHub-Hosted vs Self-Hosted Runners

| Feature | GitHub-Hosted | Self-Hosted |
|---|---|---|
| Who manages it | GitHub | User / Organization |
| Cost | Free for public repos (limited minutes for private) | You pay for your own infrastructure |
| Pre-installed tools | Many tools already installed | Must install manually |
| Good for | Simple CI pipelines | Custom environments / internal infrastructure |
| Security concern | Code runs on GitHub infrastructure | Must secure your own machine |

---

# Repository Structure

```
github-actions-practice
│
├── .github
│   └── workflows
│       ├── hosted-runners.yml
│       ├── self-hosted.yml
│
└── README.md
```

---

# Key Learnings

1. Every GitHub Actions job runs on a **runner**.
2. GitHub-hosted runners are **managed by GitHub and created on demand**.
3. Self-hosted runners allow workflows to run on **your own machines or cloud servers**.
4. Labels help target **specific runners when multiple machines are registered**.
5. Self-hosted runners are useful for **custom environments, internal tools, or specialized hardware**.

---

# Practice Repository

All workflows and experiments are available here:

https://github.com/Malviya-Shashank/github-actions-practice

---

# Tags

`90DaysOfDevOps`  
`DevOps`  
`GitHubActions`  
`CICD`  
`TrainWithShubham`