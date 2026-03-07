# Day 31 – Dockerfile: Build Your Own Images

## Overview

Today I learned how to **build custom Docker images using Dockerfiles**.  
While running containers from public images is useful, real DevOps work requires creating **custom images tailored to applications**.

A **Dockerfile** is a script that contains instructions for building a Docker image automatically.

Using Dockerfiles allows teams to:
- Standardize environments
- Automate builds
- Ensure consistent deployments across environments

---

# Task 1 – Create Your First Docker Image

## Project Structure

Created a directory for the project.

```
my-first-image/
 ├── Dockerfile
```

---

## Dockerfile

Created a Dockerfile using **Ubuntu as the base image**.

```dockerfile
FROM ubuntu:latest

RUN apt-get update && apt-get install -y curl

CMD ["echo", "Hello from my custom image!"]
```

### Explanation

| Instruction | Purpose |
|------|------|
| FROM | Defines the base image |
| RUN | Executes commands during image build |
| CMD | Default command executed when container starts |

---

## Build the Image

```bash
docker build -t my-ubuntu:v1 .
```

Explanation:

| Part | Meaning |
|-----|--------|
| docker build | Build image |
| -t | Tag image |
| my-ubuntu:v1 | Image name and version |
| . | Build context |

---

## Run the Container

```bash
docker run my-ubuntu:v1
```

Output:

```
Hello from my custom image!
```

This confirms the custom image works successfully.

---

# Task 2 – Dockerfile Instructions

Created another Dockerfile using multiple instructions.

```
docker-instructions/
 ├── Dockerfile
 ├── app.txt
```

---

## Dockerfile

```dockerfile
FROM ubuntu:latest

WORKDIR /app

COPY app.txt .

RUN apt-get update && apt-get install -y curl

EXPOSE 8080

CMD ["cat", "app.txt"]
```

---

## Explanation of Instructions

| Instruction | Description |
|------|------|
| FROM | Defines base image |
| WORKDIR | Sets working directory inside container |
| COPY | Copies files from host to image |
| RUN | Executes commands while building image |
| EXPOSE | Documents container port |
| CMD | Default command executed at runtime |

---

## Build the Image

```bash
docker build -t docker-instructions:v1 .
```

---

## Run the Container

```bash
docker run docker-instructions:v1
```

The container prints the content of `app.txt`.

---

# Task 3 – CMD vs ENTRYPOINT

Understanding the difference between **CMD** and **ENTRYPOINT** is important for controlling container behavior.

---

## Using CMD

Dockerfile:

```dockerfile
FROM alpine

CMD ["echo", "hello"]
```

Build image:

```bash
docker build -t cmd-example .
```

Run container:

```bash
docker run cmd-example
```

Output:

```
hello
```

Run with custom command:

```bash
docker run cmd-example ls
```

The CMD instruction gets **overridden**.

---

## Using ENTRYPOINT

Dockerfile:

```dockerfile
FROM alpine

ENTRYPOINT ["echo"]
```

Build image:

```bash
docker build -t entrypoint-example .
```

Run container:

```bash
docker run entrypoint-example hello
```

Output:

```
hello
```

Here, the arguments are passed to ENTRYPOINT.

---

## CMD vs ENTRYPOINT Comparison

| Feature | CMD | ENTRYPOINT |
|------|------|------|
| Purpose | Default command | Fixed executable |
| Overridable | Yes | No (unless using `--entrypoint`) |
| Best Use | Default behavior | Containers acting like commands |

---

# Task 4 – Build a Simple Web App Image

Created a simple static web page.

## Project Structure

```
my-website/
 ├── Dockerfile
 └── index.html
```

---

## index.html

```html
<!DOCTYPE html>
<html>
<head>
<title>My Docker Website</title>
</head>
<body>
<h1>Hello from my Docker container!</h1>
<p>This page is served using Nginx.</p>
</body>
</html>
```

---

## Dockerfile

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html
```

---

## Build the Image

```bash
docker build -t my-website:v1 .
```

---

## Run the Container

```bash
docker run -d -p 8080:80 my-website:v1
```

Access in browser:

```
http://localhost:8080
```

The custom web page loads successfully.

---

# Task 5 – .dockerignore

Created a `.dockerignore` file to exclude unnecessary files from the build context.

```
node_modules
.git
*.md
.env
```

### Why .dockerignore Is Important

It helps:

- Reduce build context size
- Improve build speed
- Avoid leaking secrets
- Keep images clean

---

# Task 6 – Docker Build Optimization

Docker uses **layer caching** to speed up builds.

Example:

When rebuilding an image, Docker reuses layers that have not changed.

```
Layer 1 – Base image
Layer 2 – Install packages
Layer 3 – Copy application code
```

If only the application code changes, Docker rebuilds **only the last layer**.

---

## Why Layer Order Matters

Best practice:

```
1. Base image
2. Install dependencies
3. Copy application code
```

This ensures that dependency installation is cached and **only application code layers rebuild**, making builds much faster.

---

# What I Learned Today

Today I learned:

- How Dockerfiles are used to create custom images
- Important Dockerfile instructions
- The difference between CMD and ENTRYPOINT
- How to package a web application inside a container
- The role of `.dockerignore`
- How Docker build caching works

This knowledge is essential for **building production-ready container images**.

---

# Reflection

Dockerfiles allow developers and DevOps engineers to automate application packaging and ensure consistent deployments.

By mastering Dockerfile instructions and build optimization techniques, it becomes easier to create efficient, portable container images for modern cloud-native applications.

---

# Tags

#90DaysOfDevOps  
#DevOpsKaJosh  
#TrainWithShubham  
#Docker  
#Dockerfile  
#Containerization  
#DevOpsJourney