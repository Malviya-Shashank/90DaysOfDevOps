# Day 35 – Multi-Stage Builds & Docker Hub

## Objective

The goal of this task was to learn how to build **optimized Docker images using multi-stage builds** and publish them to **Docker Hub**.

Multi-stage builds help reduce image size by separating the **build environment** from the **runtime environment**.  
This results in smaller, faster, and more secure container images.

Docker Hub allows us to **store, share, and distribute container images globally**.

---

# Task 1 – The Problem with Large Images

A simple **Node.js application** was created.

## app.js

```javascript
const http = require("http");

const server = http.createServer((req, res) => {
  res.write("Hello from Node.js Docker App 🚀");
  res.end();
});

server.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

---

## Single Stage Dockerfile

```
FROM node:20

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node","app.js"]
```

---

## Build the Image

```
docker build -t node-single-stage .
```

Check image size:

```
docker images
```

Example output:

```
node-single-stage   latest   ~1GB
```

The image is large because it includes:

- Node runtime
- build tools
- development dependencies
- unnecessary system libraries

---

# Task 2 – Multi-Stage Build

To optimize the image, a **multi-stage Dockerfile** was created.

The first stage builds the application and installs dependencies.  
The second stage copies only the required files into a **minimal runtime image**.

---

## Final Multi-Stage Dockerfile

```
# ---------- Stage 1 : Builder ----------
FROM node:20-alpine As builder

WORKDIR /app

COPY package.json .

RUN npm install --omit=dev

COPY . .

# ---------- Stage 2 : Distroless Runtime ----------
FROM gcr.io/distroless/nodejs18-debian11

WORKDIR /app

COPY --from=builder /app /app

# Run as non-root user
USER nonroot

EXPOSE 3000

CMD ["app.js"]
```

---

## Build Multi-Stage Image

```
docker build -t node-multistage .
```

Check the image size again:

```
docker images
```

Example output:

```
node-multistage   latest   ~120MB
```

---

# Image Size Comparison

| Image Type | Size |
|------------|------|
| Single Stage | ~1GB |
| Multi-Stage | ~120MB |

---

# Why Multi-Stage Images Are Smaller

Multi-stage builds reduce image size because:

- Build dependencies are removed
- Only the compiled application is copied
- Minimal runtime images are used
- Unnecessary tools are excluded

Benefits include:

- Faster container startup
- Reduced storage usage
- Improved security
- Faster image downloads in CI/CD pipelines

---

# Task 3 – Push Image to Docker Hub

Login to Docker Hub:

```
docker login
```

---

## Tag the Image

Images must follow this format:

```
username/repository:tag
```

Example:

```
docker tag node-multistage username/node-multistage-app:v1
```

---

## Push the Image

```
docker push username/node-multistage-app:v1
```

Docker will upload the image layers to Docker Hub.

---

## Verify by Pulling the Image

Remove the local image:

```
docker rmi username/node-multistage-app:v1
```

Pull it again:

```
docker pull username/node-multistage-app:v1
```

If the image downloads successfully, the push was successful.

---

# Task 4 – Docker Hub Repository

After pushing the image:

1. Open Docker Hub
2. Navigate to your repository
3. Add a repository description
4. Check the **Tags** section

Example tags:

```
v1
latest
```

---

## Pull Specific Version

```
docker pull username/node-multistage-app:v1
```

## Pull Latest Version

```
docker pull username/node-multistage-app:latest
```

Using specific tags helps manage versions and prevents unexpected updates.

---

# Task 5 – Image Best Practices

Several Docker best practices were applied:

### Use Minimal Base Images

```
node:20-alpine
```

Alpine images are much smaller than full Linux distributions.

---

### Run Containers as Non-Root

```
USER nonroot
```

Running containers as non-root improves security and prevents privilege escalation.

---

### Reduce Image Layers

Combine commands when possible to reduce layers and optimize builds.

---

### Use Specific Image Tags

Avoid:

```
FROM node:latest
```

Use:

```
FROM node:20-alpine
```

This ensures predictable builds.

---

# Commands Used

Build image

```
docker build -t image-name .
```

Run container

```
docker run -p 3000:3000 image-name
```

Tag image

```
docker tag image-name username/repo:tag
```

Push image

```
docker push username/repo:tag
```

Pull image

```
docker pull username/repo:tag
```

List images

```
docker images
```

---

# Key Learnings

From this task:

- Multi-stage builds significantly reduce Docker image size
- Distroless images improve container security
- Docker Hub allows sharing container images
- Image tagging helps manage versions
- Running containers as non-root improves security

---

# Tags

#Docker  
#DockerHub  
#MultiStageBuild  
#Containers  
#DevOps  
#TrainWithShubham