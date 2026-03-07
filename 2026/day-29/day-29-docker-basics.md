# Day 29 – Introduction to Docker

## Overview

Today I started learning **Docker**, one of the most important tools in modern DevOps. Docker allows developers and operations teams to package applications along with their dependencies into lightweight, portable containers that run consistently across environments.

Containers solve the classic problem of **“It works on my machine but not on production.”**

Docker makes it easier to build, ship, and run applications reliably.

---

# Task 1 – What is Docker?

## What is a Container?

A **container** is a lightweight, standalone package that includes:
- Application code
- Runtime environment
- System libraries
- Dependencies
- Configuration files

Containers ensure that the application runs the same way regardless of the environment.

Example:
- A developer builds an application in a container.
- The same container runs in testing, staging, and production without modification.

### Why Containers Are Needed

Before containers, applications were installed directly on servers, which caused issues such as:

- Dependency conflicts
- Different environment configurations
- Difficult deployments
- Harder scaling

Containers solve these problems by **isolating applications and their dependencies.**

---

# Containers vs Virtual Machines

| Feature | Containers | Virtual Machines |
|-------|------------|----------------|
| Virtualization Type | OS-level virtualization | Hardware virtualization |
| Boot Time | Seconds | Minutes |
| Resource Usage | Lightweight | Heavy |
| OS Included | No | Yes |
| Isolation | Process-level | Full OS isolation |

### Virtual Machine Architecture

VMs require a **hypervisor** and run full operating systems.

```
Hardware
   │
Hypervisor
   │
Guest OS
   │
Applications
```

### Container Architecture

Containers share the host OS kernel.

```
Hardware
   │
Host OS
   │
Docker Engine
   │
Containers
```

This makes containers **much faster and more efficient**.

---

# Docker Architecture

Docker uses a **client-server architecture**.

### Components

1️⃣ **Docker Client**

The command-line interface used to interact with Docker.

Example:

```bash
docker run nginx
```

The client sends commands to the Docker daemon.

---

2️⃣ **Docker Daemon**

The Docker daemon (`dockerd`) runs in the background and manages:

- Containers
- Images
- Networks
- Volumes

It performs the actual container operations.

---

3️⃣ **Docker Images**

A **Docker image** is a read-only template used to create containers.

Example images:

- nginx
- ubuntu
- redis
- node

Images contain everything needed to run the application.

---

4️⃣ **Docker Containers**

A **container** is a running instance of an image.

Example:

```
docker run nginx
```

This command:
- Downloads the nginx image
- Creates a container
- Starts the container

---

5️⃣ **Docker Registry**

A **registry** stores Docker images.

The most popular registry is **Docker Hub**.

Example:

```
docker pull nginx
```

This downloads the nginx image from Docker Hub.

---

# Docker Architecture Summary

```
Developer
   │
Docker Client
   │
Docker Daemon
   │
Images  →  Containers
   │
Docker Hub (Registry)
```

---

# Task 2 – Install Docker

### Install Docker

Docker was installed on the system using the official package.

---

### Verify Docker Installation

```bash
docker --version
```

Example output:

```
Docker version 24.x.x
```

---

### Run Hello World Container

```bash
docker run hello-world
```

This command performs several steps:

1. Docker checks for the image locally
2. If not found, it downloads it from Docker Hub
3. Creates a container
4. Runs the container
5. Displays a confirmation message

Example output:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

---

# Task 3 – Run Real Containers

## Run Nginx Container

```bash
docker run -d -p 8080:80 nginx
```

Explanation:

| Option | Meaning |
|------|------|
| -d | Run container in detached mode |
| -p | Map host port to container port |
| nginx | Docker image name |

Access the container in browser:

```
http://localhost:8080
```

The Nginx welcome page confirms the container is running.

---

## Run Ubuntu Container in Interactive Mode

```bash
docker run -it ubuntu
```

Explanation:

| Option | Meaning |
|------|------|
| -i | Interactive mode |
| -t | Terminal session |

Inside the container:

```bash
ls
apt update
cat /etc/os-release
```

This allows exploring the container like a small Linux environment.

---

## List Running Containers

```bash
docker ps
```

Example output:

```
CONTAINER ID   IMAGE   STATUS   PORTS
abc123         nginx   Up       0.0.0.0:8080->80
```

---

## List All Containers

```bash
docker ps -a
```

Shows both running and stopped containers.

---

## Stop a Container

```bash
docker stop <container_id>
```

Example:

```bash
docker stop abc123
```

---

## Remove a Container

```bash
docker rm <container_id>
```

Example:

```bash
docker rm abc123
```

---

# Task 4 – Explore Docker Features

## Run Container in Detached Mode

```bash
docker run -d nginx
```

Detached mode runs the container **in the background**.

---

## Give a Container a Custom Name

```bash
docker run -d --name my-nginx nginx
```

Now the container can be referenced by name instead of ID.

---

## Port Mapping

```bash
docker run -d -p 8080:80 nginx
```

This maps:

```
Host Port → Container Port
8080 → 80
```

---

## View Container Logs

```bash
docker logs <container_name>
```

Example:

```bash
docker logs my-nginx
```

Logs help debug container issues.

---

## Run Commands Inside a Container

```bash
docker exec -it my-nginx bash
```

This opens a shell inside the running container.

Example commands inside container:

```bash
ls
ps aux
```

---

# Why Docker is Important for DevOps

Docker plays a critical role in modern DevOps workflows because it enables:

- **Consistent environments**
- **Faster deployments**
- **Microservice architectures**
- **CI/CD pipelines**
- **Scalable cloud-native applications**

Docker is also the **foundation of Kubernetes**, which is widely used for container orchestration.

---

# What I Learned Today

Today I learned:

- What containers are and why they are important
- The difference between containers and virtual machines
- Docker architecture and its components
- How to install Docker
- How to run and manage containers
- How to map ports and access services from containers
- How to inspect logs and run commands inside containers

This was my **first step into containerization**, which is a core skill in modern DevOps engineering.

---

# Reflection

Learning Docker helps bridge the gap between **development and operations**. Containers make applications portable, reproducible, and easier to manage in production environments.

This knowledge will be useful for upcoming topics like:

- Dockerfiles
- Docker Compose
- Container networking
- Kubernetes

---

# Tags

#90DaysOfDevOps  
#DevOpsKaJosh  
#TrainWithShubham  
#Docker  
#Containerization  
#DevOpsJourney  
#CloudNative