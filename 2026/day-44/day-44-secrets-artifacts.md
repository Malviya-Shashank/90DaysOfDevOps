# Day 44 – Secrets, Artifacts & Running Real Tests in CI

## Objective

The goal of Day 44 was to make the CI pipeline do **real-world tasks**, including:

- Managing sensitive data using GitHub Secrets
- Storing and sharing files using artifacts
- Running actual scripts/tests in CI
- Improving performance using caching

Repository used for practice:

https://github.com/shashankmalv/github-actions-practice

---

# Task 1 – GitHub Secrets

I created a secret in GitHub:

```
MY_SECRET_MESSAGE
```

### Workflow

```yaml
name: Secrets Demo

on:
  push:

jobs:
  secret-job:
    runs-on: ubuntu-latest

    steps:
      - name: Check secret
        run: |
          if [ -n "${{ secrets.MY_SECRET_MESSAGE }}" ]; then
            echo "The secret is set: true"
          else
            echo "The secret is not set"
          fi

      - name: Try printing secret
        run: echo "${{ secrets.MY_SECRET_MESSAGE }}"
```

### Result

- GitHub **masked the secret value** in logs (shown as `***`)
- The actual value was never exposed

### Notes

Secrets should never be printed because:

- Logs are visible to others
- Sensitive data like API keys can be leaked
- Security risk for production systems

---

# Task 2 – Using Secrets as Environment Variables

I passed secrets securely into a step.

```yaml
- name: Use secret as env variable
  env:
    SECRET_VALUE: ${{ secrets.MY_SECRET_MESSAGE }}
  run: echo "Using secret securely"
```

### Additional Secrets Added

```
DOCKER_USERNAME
DOCKER_TOKEN
```

### Result

- Secrets were used without hardcoding
- Values remained hidden in logs

---

# Task 3 – Upload Artifacts

I created a file and uploaded it as an artifact.

```yaml
- name: Create file
  run: echo "This is a test artifact" > report.txt

- name: Upload artifact
  uses: actions/upload-artifact@v4
  with:
    name: test-report
    path: report.txt
```

### Result

- Artifact was visible in the **Actions tab**
- Successfully downloaded from GitHub UI

---

# Task 4 – Download Artifacts Between Jobs

I shared a file between two jobs.

```yaml
jobs:

  create-artifact:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Artifact data" > data.txt

      - uses: actions/upload-artifact@v4
        with:
          name: shared-file
          path: data.txt

  use-artifact:
    runs-on: ubuntu-latest
    needs: create-artifact

    steps:
      - uses: actions/download-artifact@v4
        with:
          name: shared-file

      - run: cat data.txt
```

### Result

- File created in Job 1 was successfully used in Job 2

### Use Case

Artifacts are used for:

- Sharing build outputs
- Passing logs/reports
- Deployable packages between stages

---

# Task 5 – Running Real Tests in CI

I ran a real script inside the CI pipeline.

### Example Python Script

```python
print("Hello from CI test script")
```

### Workflow

```yaml
name: Run Tests

on:
  push:

jobs:
  test-job:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.11"

      - name: Run script
        run: python test.py
```

### Result

- Script executed successfully → pipeline turned **green**
- When script was broken → pipeline turned **red**
- Fixing script → pipeline became **green again**

---

# Task 6 – Caching

I used caching to speed up dependency installation.

```yaml
- name: Cache dependencies
  uses: actions/cache@v4
  with:
    path: ~/.cache/pip
    key: ${{ runner.os }}-pip-${{ hashFiles('**/requirements.txt') }}
    restore-keys: |
      ${{ runner.os }}-pip-

- name: Install dependencies
  run: pip install -r requirements.txt
```

### Result

- First run → slow (no cache)
- Second run → faster (cache hit)

### Notes

Caching:

- Stores dependencies between runs
- Reduces execution time
- Stored on GitHub infrastructure

---

# Repository Structure

```
github-actions-practice
│
├── .github
│   └── workflows
│       ├── secrets.yml
│       ├── artifacts.yml
│       ├── test.yml
│       └── cache.yml
│
├── test.py
└── README.md
```

---

# Key Learnings

1. GitHub Secrets securely store sensitive data like tokens and credentials.
2. Secrets should never be printed in logs.
3. Artifacts help store and share files between jobs.
4. CI pipelines can run real tests and validate code automatically.
5. Caching significantly improves pipeline performance.

---

# Practice Repository

All workflows and experiments are available here:

https://github.com/shashankmalv/github-actions-practice

---

# Tags

#90DaysOfDevOps  
#DevOps  
#GitHubActions  
#CICD  
#TrainWithShubham