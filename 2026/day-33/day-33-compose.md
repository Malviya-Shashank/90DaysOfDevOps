# Day 33 – Docker Compose: Multi-Container Basics

## Overview
Today’s goal was to learn **Docker Compose**, a tool that allows us to run multi-container applications using a single configuration file.

Instead of manually creating networks, volumes, and containers, Docker Compose defines everything in a **YAML file** and manages the application lifecycle with simple commands.

---

# Task 1 – Install & Verify Docker Compose

### Check if Docker Compose is Installed

```bash
docker compose version
```

### Example Output

```
Docker Compose version v2.x.x
```

### Explanation

Docker Compose is now included with Docker Desktop and modern Docker installations.  
It allows us to manage **multi-container applications using a YAML configuration file**.

---

# Task 2 – Your First Compose File

### Create Project Folder

```bash
mkdir compose-basics
cd compose-basics
```

### Create `docker-compose.yml`

```yaml
version: "3"

services:
  web:
    image: nginx
    ports:
      - "8080:80"
```

### Start the Container

```bash
docker compose up
```

### Start in Detached Mode

```bash
docker compose up -d
```

### Open Browser

```
http://localhost:8080
```

You should see the **Nginx welcome page**.

### Stop Containers

```bash
docker compose down
```

### Explanation

Docker Compose reads the YAML file and automatically:

- Pulls the image
- Creates the container
- Configures networking
- Starts the service

---

# Task 3 – Two-Container Setup (WordPress + MySQL)

Now we create a real-world **multi-container application**.

### Create `docker-compose.yml`

```yaml
version: "3"

services:

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wppass
    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    image: wordpress
    restart: always
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppass
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db

volumes:
  db_data:
```

### Start the Application

```bash
docker compose up -d
```

### Open WordPress

```
http://localhost:8080
```

Follow the WordPress setup wizard.

### Verify Data Persistence

Stop the stack:

```bash
docker compose down
```

Start it again:

```bash
docker compose up -d
```

Result:  
Your **WordPress data is still there** because the MySQL database uses a **named volume**.

### Explanation

Docker Compose automatically creates:

- A **network** for all services
- DNS resolution between containers
- Persistent **volumes** if defined

WordPress connects to MySQL using the **service name `db`**.

---

# Task 4 – Common Docker Compose Commands

### Start Services (Detached Mode)

```bash
docker compose up -d
```

### View Running Containers

```bash
docker compose ps
```

### View Logs of All Services

```bash
docker compose logs
```

### Follow Logs in Real Time

```bash
docker compose logs -f
```

### View Logs of Specific Service

```bash
docker compose logs wordpress
```

### Stop Services (Without Removing)

```bash
docker compose stop
```

### Remove Everything (Containers + Network)

```bash
docker compose down
```

### Rebuild Containers

```bash
docker compose up --build
```

---

# Task 5 – Environment Variables

Environment variables allow us to configure applications without hardcoding values.

### Option 1 – Directly in Compose File

```yaml
environment:
  MYSQL_ROOT_PASSWORD: rootpass
```

### Option 2 – Using `.env` File

Create a `.env` file:

```
MYSQL_ROOT_PASSWORD=rootpass
MYSQL_DATABASE=wordpress
MYSQL_USER=wpuser
MYSQL_PASSWORD=wppass
```

Update Compose file:

```yaml
environment:
  MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
  MYSQL_DATABASE: ${MYSQL_DATABASE}
  MYSQL_USER: ${MYSQL_USER}
  MYSQL_PASSWORD: ${MYSQL_PASSWORD}
```

### Verify Variables

Run:

```bash
docker compose config
```

This shows the final configuration after variables are substituted.

---

# Key Learnings

### Docker Compose Simplifies Multi-Container Applications

Instead of running many `docker run` commands, we define everything in one YAML file.

### Automatic Networking

All services in a Compose file share a **default network**, allowing containers to communicate using **service names**.

### Persistent Data with Volumes

Named volumes ensure that database data is not lost when containers restart.

### Environment Variables Improve Configuration

Using `.env` files makes Compose configurations **cleaner and more secure**.

---

# Screenshots

Add screenshots for:

- Running `docker compose ps`
- WordPress setup page
- `docker compose logs`
- WordPress running after restart

---

# Conclusion

Docker Compose allows developers and DevOps engineers to **define and run multi-container applications easily**.

Using a single configuration file, we can manage:

- Multiple containers
- Networks
- Volumes
- Environment variables

This approach is essential when deploying **real-world microservices applications** using Docker.

---

# Submission

Add files to the repository:

```
2026/day-33/
 ├── day-33-compose.md
 ├── docker-compose.yml
 └── .env
```

Push changes:

```bash
git add .
git commit -m "Day 33 - Docker Compose Multi-Container Setup"
git push
```

---

# Learn in Public

Example LinkedIn post:

> Day 33 of #90DaysOfDevOps  
>
> Today I learned how Docker Compose simplifies running multi-container applications.
>
> I deployed a WordPress + MySQL stack using a single `docker-compose.yml` file.
>
> Compose automatically handled networking, volumes, and container orchestration.
>
> DevOps is getting more interesting every day 🚀
>
> #DevOps #Docker #TrainWithShubham