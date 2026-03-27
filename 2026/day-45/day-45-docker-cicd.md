# Day 45 – Docker Build & Push in GitHub Actions

## Objective

The goal of Day 45 was to build a **complete CI/CD pipeline** where:

- Code triggers GitHub Actions workflow
- Docker image is built automatically
- Image is pushed to Docker Hub
- No manual steps required

Repository used for practice:

https://github.com/shashankmalv/github-actions-practice

---

# Task 1 – Preparation

I used a simple Docker application and added the **Dockerfile** to my repository.

### Sample Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY . .

CMD ["python", "test.py"]
```

### Secrets & Variables Used

```
DOCKER_TOKEN  (GitHub Secret)
DOCKER_USERNAME (GitHub Variable)
```

---

# Task 2 & 3 – Build and Push Docker Image

I created the workflow file:

```
.github/workflows/docker-publish.yml
```

### docker-publish.yml

```yaml
name: docker-publish

on:
  workflow_dispatch
  # push:
  #   branches: [main]

jobs:
  docker:
    runs-on: ubuntu-latest

    steps:
      - name: Checkoout code
        uses: actions/checkout@v4

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_TOKEN }}

      - name: Set short SHA
        run: echo "SHORT_SHA=${GITHUB_SHA::7}" >> $GITHUB_ENV

      - name: Build & Push
        uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          tags: |
            ${{ vars.DOCKER_USERNAME }}/github-actions-app:latest
            ${{ vars.DOCKER_USERNAME }}/github-actions-app:sha-${{ env.SHORT_SHA }}
```

---

# Result

- Docker image successfully built in CI
- Image pushed to Docker Hub
- Two tags created:
  - `latest`
  - `sha-<commit>`

---

# Task 4 – Trigger Behavior

Currently using:

```
workflow_dispatch
```

This allows **manual execution from GitHub Actions UI**.

### Optional

To enable automatic execution on push:

```yaml
on:
  push:
    branches: [main]
```

---

# Task 5 – Status Badge

### Badge URL

```
https://github.com/shashankmalv/github-actions-practice/actions/workflows/docker-publish.yml/badge.svg
```

### Add to README

```md
![Docker CI](https://github.com/shashankmalv/github-actions-practice/actions/workflows/docker-publish.yml/badge.svg)
```

---

# Task 6 – Pull and Run the Image

### Commands

```bash
docker pull <your-docker-username>/github-actions-app:latest

docker run <your-docker-username>/github-actions-app:latest
```

### Result

- Container ran successfully
- Application output verified

---

# Full CI/CD Flow

```
Manual trigger → GitHub Actions →
Checkout code → Login to Docker Hub →
Build image → Tag image →
Push to Docker Hub → Pull image → Run container
```

---

# Repository Structure

```
github-actions-practice
│
├── .github
│   └── workflows
│       └── docker-publish.yml
│
├── Dockerfile
├── test.py
└── README.md
```

---

# Key Learnings

1. GitHub Actions can fully automate Docker build and push.
2. Secrets and variables help securely manage credentials.
3. `docker/build-push-action` simplifies Docker workflows.
4. Tagging with commit SHA improves traceability.
5. Manual triggers (`workflow_dispatch`) are useful for controlled deployments.

---

# Practice Repository

All workflows and code are available here:

https://github.com/shashankmalv/github-actions-practice

---

# Tags

#90DaysOfDevOps  
#DevOps  
#Docker  
#GitHubActions  
#CICD  
#TrainWithShubham