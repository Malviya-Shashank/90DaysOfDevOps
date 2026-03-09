# Day 36 – Docker Project: Dockerize a Full Application

## Overview

Today’s goal was to **Dockerize a real application end-to-end**.

Instead of following a tutorial, the task was to simulate a **real-world DevOps workflow**:
- Take an application
- Containerize it
- Run it with a database
- Use Docker Compose
- Push the image to Docker Hub
- Verify everything works from scratch

This is exactly how containerized applications are shipped and deployed in production environments.

---

# Application Chosen

For this project, I chose a:

**Node.js Express application with MongoDB**

### Why I chose this application

1. It represents a **typical backend service**
2. It requires a **database dependency**
3. It demonstrates **multi-container architecture**
4. It is a common stack used in real production systems

The application:
- Runs a Node.js Express server
- Connects to MongoDB
- Returns a response from an API endpoint

---

# Project Structure

```
node-express-mongodb/
│
├── Dockerfile
├── docker-compose.yml
├── .env
├── .dockerignore
├── package.json
├── server.js
└── README.md
```

This structure separates:
- **application code**
- **container configuration**
- **environment configuration**

---

# Task 1 – Selecting the Application

### What I did

I selected a **Node.js Express application connected to MongoDB**.

### Why this is important

In real DevOps workflows, engineers often need to:

- containerize **existing applications**
- integrate **databases**
- run **multi-container systems**

This task simulates the real scenario of taking an application and making it **portable and reproducible using Docker**.

---

# Task 2 – Writing the Dockerfile

The Dockerfile defines **how the application image is built**.

### Dockerfile

```dockerfile
# Use a lightweight Node.js base image
FROM node:20-alpine

# Set working directory inside container
WORKDIR /app

# Copy dependency files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy application source code
COPY . .

# Expose the application port
EXPOSE 8080

# Start the application
CMD ["node", "server.js"]
```

---

## Explanation of Each Step

### `FROM node:20-alpine`

This sets the base image.

Why Alpine?

- Smaller image size
- Faster download
- Reduced security surface

Smaller images are **very important in production** because they:
- deploy faster
- consume less storage
- reduce attack surface.

---

### `WORKDIR /app`

Sets the working directory inside the container.

Why this matters:

Without this, files would be copied into the container root, which is messy and hard to maintain.

---

### `COPY package*.json ./`

We copy only dependency files first.

Why?

Docker caches layers.

This means:

If dependencies don't change, Docker **won't reinstall them**, making builds **much faster**.

---

### `RUN npm install`

Installs project dependencies.

This step prepares the runtime environment for the application.

---

### `COPY . .`

Copies the rest of the application files.

---

### `EXPOSE 8080`

Documents the port used by the container.

It doesn't publish the port but helps tools understand the service port.

---

### `CMD ["node", "server.js"]`

Defines the default command used to start the application.

---

# .dockerignore

This file prevents unnecessary files from being included in the image.

Example:

```
node_modules
.git
.gitignore
Dockerfile
README.md
.env
```

### Why this matters

Without `.dockerignore`:

- images become larger
- builds become slower
- unnecessary files leak into containers

---

# Task 3 – Docker Compose Setup

Docker Compose allows running **multiple containers together**.

Our application requires:

- Node.js application container
- MongoDB database container

---

## docker-compose.yml

```yaml
version: "3.9"

services:

  app:
    image: shashank2034/node-express-mongodb-app:v1
    container_name: node-app
    ports:
      - "8080:8080"
    env_file:
      - .env
    depends_on:
      mongo:
        condition: service_healthy
    networks:
      - app-network

  mongo:
    image: mongo:6
    container_name: mongodb
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db
    networks:
      - app-network
    healthcheck:
      test: ["CMD", "mongosh", "--eval", "db.adminCommand('ping')"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  mongo-data:

networks:
  app-network:
    driver: bridge
```

---

## Why Docker Compose is Important

In real systems applications rarely run alone.

Most applications require:

- databases
- caches
- background workers
- APIs

Docker Compose allows defining the **entire application stack in one file**.

---

# Environment Variables (.env)

```
PORT=8080
MONGO_HOST=mongo
MONGO_PORT=27017
MONGO_DB=devopsdb
```

### Why environment variables are important

They allow configuration **without changing code**.

Examples:

- database credentials
- ports
- API keys
- environment-specific settings

This follows the **12-factor app principle**.

---

# Health Checks

The MongoDB container includes a health check.

```
healthcheck:
  test: ["CMD", "mongosh", "--eval", "db.adminCommand('ping')"]
```

### Why this matters

Without health checks:

The application might start **before the database is ready**, causing connection failures.

Health checks ensure containers start **in the correct order**.

---

# Task 4 – Shipping the Image

After building the image locally, I pushed it to **Docker Hub**.

### Build Image

```
docker build -t shashank2034/node-express-mongodb-app:v1 .
```

### Push to Docker Hub

```
docker push shashank2034/node-express-mongodb-app:v1
```

---

# Docker Hub Image

Docker Hub repository:

```
https://hub.docker.com/r/shashank2034/node-express-mongodb-app
```

This allows anyone to run the application without building it locally.

---

# Task 5 – Testing From Scratch

To verify the setup works on a fresh machine, I removed everything.

### Remove Containers

```
docker rm -f $(docker ps -aq)
```

### Remove Images

```
docker rmi $(docker images -q)
```

### Run Again

```
docker compose up
```

Docker automatically:

1. Pulled the image from Docker Hub
2. Started MongoDB
3. Started the Node application
4. Connected both services through the network

This confirms the setup is **fully reproducible**.

---

# Final Result

Application runs at:

```
http://localhost:8080
```

The stack includes:

- Node.js Express API
- MongoDB database
- Docker containers
- Docker Compose orchestration

---

# Final Image Size

The image was kept relatively small by using:

```
node:20-alpine
```

Using Alpine significantly reduces container size compared to full Linux images.

---

# Challenges Faced

### 1. MongoDB connection issues

Problem:

Application could not connect to MongoDB.

Solution:

Use the **service name as hostname**:

```
mongodb://mongo:27017/dbname
```

Containers communicate using **service names**, not `localhost`.

---

### 2. Container startup order

Problem:

App started before database was ready.

Solution:

Added **healthcheck and depends_on** in Docker Compose.

---

### 3. Image tagging errors

Problem:

Incorrect tagging format.

Solution:

Use proper syntax:

```
docker tag source-image target-image:tag
```

---

# Key DevOps Concepts Learned

This project reinforced several important DevOps principles:

### Containerization
Packaging applications and dependencies together.

### Reproducibility
Applications run the same everywhere.

### Infrastructure as Code
Application environment defined in configuration files.

### Service Isolation
Each component runs in its own container.

### Scalability
Containers can easily be scaled horizontally.

---

# Conclusion

This project demonstrated how to:

- containerize a backend application
- run it with a database
- orchestrate services using Docker Compose
- publish container images
- verify the entire workflow

These are **real-world DevOps skills** used in production environments.

---

# Tags

`90DaysOfDevOps`  `DevOpsKaJosh`  `TrainWithShubham`

