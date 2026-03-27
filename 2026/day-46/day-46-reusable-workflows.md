# Day 46 – Reusable Workflows & Composite Actions

## Objective

The goal of Day 46 was to learn how to **avoid duplication in CI/CD pipelines** by using:

- Reusable workflows (`workflow_call`)
- Composite actions (custom reusable steps)

Repository used for practice:

https://github.com/shashankmalv/github-actions-practice

---

# Task 1 – Understanding Reusable Workflows

### What is a reusable workflow?

A reusable workflow is a **predefined GitHub Actions workflow** that can be called by other workflows, similar to a function.

### What is `workflow_call`?

`workflow_call` is a trigger that allows a workflow to be executed by another workflow.

### Difference from regular action (`uses:`)

- Reusable workflow → used at **job level**
- Action → used at **step level**

### File location

Reusable workflows must be placed inside:

```
.github/workflows/
```

---

# Task 2 – Create Reusable Workflow

Workflow file:

```
.github/workflows/reusable-build.yml
```

### reusable-build.yml

```yaml
name: Reusable Build Workflow

on:
  workflow_call:
    inputs:
      app_name:
        required: true
        type: string
      environment:
        required: true
        type: string
        default: staging

    secrets:
      docker_token:
        required: true

jobs:
  build:
    runs-on: ubuntu-latest

    outputs:
      build_version: ${{ steps.set-version.outputs.version }}

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Print build info
        run: echo "Building ${{ inputs.app_name }} for ${{ inputs.environment }}"

      - name: Check secret
        run: |
          if [ -n "${{ secrets.docker_token }}" ]; then
            echo "Docker token is set: true"
          else
            echo "Docker token is missing"
          fi

      - name: Set version
        id: set-version
        run: echo "version=v1.0-${GITHUB_SHA::7}" >> $GITHUB_OUTPUT
```

---

# Task 3 – Caller Workflow

Workflow file:

```
.github/workflows/call-build.yml
```

### call-build.yml

```yaml
name: Call Reusable Workflow

on:
  push:
    branches:
      - main

jobs:
  build:
    uses: ./.github/workflows/reusable-build.yml
    with:
      app_name: "my-web-app"
      environment: "production"
    secrets:
      docker_token: ${{ secrets.DOCKER_TOKEN }}

  print-version:
    runs-on: ubuntu-latest
    needs: build

    steps:
      - name: Print build version
        run: echo "Version is ${{ needs.build.outputs.build_version }}"
```

---

# Result

- Caller workflow successfully triggered reusable workflow
- Inputs (`app_name`, `environment`) were printed
- Output (`build_version`) passed to next job successfully

---

# Task 4 – Composite Action

I created a custom composite action.

Location:

```
.github/actions/setup-and-greet/action.yml
```

### action.yml

```yaml
name: Setup and Greet

description: Custom composite action to greet user

inputs:
  name:
    description: "Name of the user"
    required: true
  language:
    description: "Language of greeting"
    required: false
    default: "en"

outputs:
  greeted:
    description: "Greeting completed"
    value: true

runs:
  using: "composite"

  steps:
    - name: Print greeting
      shell: bash
      run: |
        if [ "${{ inputs.language }}" == "en" ]; then
          echo "Hello, ${{ inputs.name }}"
        else
          echo "Namaste, ${{ inputs.name }}"
        fi

    - name: Print system info
      shell: bash
      run: |
        date
        uname -a
```

---

# Using Composite Action

Workflow file:

```
.github/workflows/composite-test.yml
```

### composite-test.yml

```yaml
name: Composite Action Test

on:
  workflow_dispatch:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Run custom action
        uses: ./.github/actions/setup-and-greet
        with:
          name: "Shashank"
          language: "en"
```

---

# Result

- Composite action executed successfully
- Greeting printed correctly
- System information displayed

---

# Reusable Workflow vs Composite Action

| Feature | Reusable Workflow | Composite Action |
|---|---|---|
| Triggered by | `workflow_call` | `uses:` in a step |
| Can contain jobs? | Yes | No |
| Can contain multiple steps? | Yes | Yes |
| Lives where? | `.github/workflows/` | `.github/actions/` |
| Can accept secrets directly? | Yes | No (passed via env) |
| Best for | Full pipeline reuse | Reusable step logic |

---

# Repository Structure

```
github-actions-practice
│
├── .github
│   ├── workflows
│   │   ├── reusable-build.yml
│   │   ├── call-build.yml
│   │   └── composite-test.yml
│   │
│   └── actions
│       └── setup-and-greet
│           └── action.yml
│
└── README.md
```

---

# Key Learnings

1. Reusable workflows help eliminate duplication across pipelines.
2. `workflow_call` allows workflows to act like functions.
3. Outputs can be passed from reusable workflows to caller workflows.
4. Composite actions simplify reusable step-level logic.
5. Separation of reusable workflows and actions improves maintainability.

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