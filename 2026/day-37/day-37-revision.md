# 🐳 Docker Cheat Sheet

A quick reference guide for **Docker commands, Dockerfile instructions, networking, volumes, and Docker Compose**.

Perfect for **DevOps engineers, developers, and interview preparation.**

---

# 📦 Docker Architecture (Quick Overview)

```
Application → Container → Image → Dockerfile → Docker Engine → Host OS
```

| Component | Description |
|----------|-------------|
| Dockerfile | Blueprint to build an image |
| Image | Immutable template used to create containers |
| Container | Running instance of an image |
| Docker Engine | Runtime that manages containers |

---

# 🚀 Container Commands

| Command | Description |
|------|-------------|
| `docker run image` | Run a container from image |
| `docker run -it image bash` | Run container interactively |
| `docker run -d image` | Run container in detached mode |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers |
| `docker stop <container>` | Stop container |
| `docker start <container>` | Start container |
| `docker restart <container>` | Restart container |
| `docker rm <container>` | Remove container |
| `docker exec -it <container> bash` | Access container shell |
| `docker logs <container>` | View logs |
| `docker inspect <container>` | Show container details |

---

# 🖼️ Image Commands

| Command | Description |
|------|-------------|
| `docker build -t image-name .` | Build image from Dockerfile |
| `docker images` | List images |
| `docker pull <image>` | Download image from Docker Hub |
| `docker push <image>` | Push image to Docker Hub |
| `docker tag image repo:tag` | Tag an image |
| `docker rmi image` | Remove image |
| `docker history image` | Show image layers |

---

# 📂 Volume Commands (Persistent Storage)

| Command | Description |
|------|-------------|
| `docker volume create myvolume` | Create volume |
| `docker volume ls` | List volumes |
| `docker volume inspect myvolume` | Inspect volume |
| `docker volume rm myvolume` | Remove volume |

### Example

```
docker run -v myvolume:/app/data nginx
```

Volumes ensure **data persists even after container deletion**.

---

# 📁 Bind Mount Example

Bind mounts map a **local directory to container directory**.

```
docker run -v $(pwd):/app node
```

| Part | Meaning |
|------|--------|
| $(pwd) | Local directory |
| /app | Container directory |

Commonly used in **development environments**.

---

# 🔗 Network Commands

| Command | Description |
|------|-------------|
| `docker network create mynetwork` | Create network |
| `docker network ls` | List networks |
| `docker network inspect mynetwork` | Inspect network |
| `docker network connect network container` | Connect container |

### Example

```
docker network create app-network

docker run -d --name db --network app-network mysql

docker run -d --name app --network app-network node
```

Containers communicate using **container names as DNS**.

---

# 🔐 Port Mapping

```
docker run -p 8080:80 nginx
```

| Part | Meaning |
|-----|--------|
| 8080 | Host port |
| 80 | Container port |

Access application:

```
http://localhost:8080
```

---

# 🐳 Docker Compose Commands

| Command | Description |
|------|-------------|
| `docker compose up` | Start services |
| `docker compose up -d` | Start in detached mode |
| `docker compose down` | Stop services |
| `docker compose down -v` | Remove volumes also |
| `docker compose ps` | List compose containers |
| `docker compose logs` | Show logs |
| `docker compose build` | Build services |

---

# ⚙️ Dockerfile Instructions

| Instruction | Purpose |
|-------------|--------|
| `FROM` | Define base image |
| `RUN` | Execute commands during build |
| `COPY` | Copy files to image |
| `ADD` | Copy files + extract archives |
| `WORKDIR` | Set working directory |
| `ENV` | Define environment variable |
| `EXPOSE` | Document container port |
| `CMD` | Default command |
| `ENTRYPOINT` | Main container command |

---

# 🧱 Example Dockerfile

```
FROM node:18

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

---

# 🏗️ Multi-Stage Docker Build

Used to **reduce image size and improve security**.

```
FROM node:18 AS builder

WORKDIR /app
COPY . .
RUN npm install

FROM node:18-alpine

WORKDIR /app
COPY --from=builder /app .

CMD ["npm","start"]
```

Benefits:

✔ Smaller image  
✔ Faster deployment  
✔ Reduced vulnerabilities  

---

# 🧹 Docker Cleanup Commands

| Command | Description |
|--------|-------------|
| `docker system df` | Show disk usage |
| `docker system prune` | Remove unused data |
| `docker container prune` | Remove stopped containers |
| `docker image prune` | Remove unused images |
| `docker volume prune` | Remove unused volumes |

---

# 📊 Docker Debugging Commands

| Command | Purpose |
|-------|--------|
| `docker logs container` | Check logs |
| `docker exec -it container sh` | Enter container |
| `docker stats` | Resource usage |
| `docker inspect container` | Metadata information |

---

# 🧠 Important Concepts (Interview)

## Image vs Container

| Image | Container |
|------|-----------|
| Blueprint | Running instance |
| Read-only | Writable runtime |
| Immutable | Temporary |

---

## CMD vs ENTRYPOINT

| CMD | ENTRYPOINT |
|----|-------------|
| Default command | Main command |
| Can be overridden | Hard to override |

Example:

```
ENTRYPOINT ["python"]
CMD ["app.py"]
```

Runs as:

```
python app.py
```

---

# 📌 Docker Best Practices

✔ Use **small base images (Alpine)**  
✔ Use **multi-stage builds**  
✔ Minimize RUN layers  
✔ Avoid running containers as root  
✔ Use `.dockerignore`  
✔ Tag images properly  
✔ Scan images for vulnerabilities  

---

# 📚 Useful Docker Commands (Quick View)

```
docker run -d nginx
docker ps
docker exec -it container bash
docker build -t myapp .
docker images
docker stop container
docker rm container
docker rmi image
docker compose up -d
docker compose down
docker system prune
```

---

# 🧠 Quick Interview Questions

1. Difference between **Docker Image and Container**
2. What happens to **container data after deletion**
3. Difference between **Volume vs Bind Mount**
4. Why use **multi-stage builds**
5. Difference between **CMD and ENTRYPOINT**
6. What does **-p 8080:80** mean
7. How containers communicate inside **Docker network**
8. What is **Docker layer caching**

---

# 🏷️ Tags

`#Docker` `#DevOps` `#Containers` 
`#90DaysOfDevOps` `#TrainWithShubham`

---

⭐ Tip: Keep this file in your GitHub repository as **docker-cheatsheet.md** for quick revision before DevOps interviews.