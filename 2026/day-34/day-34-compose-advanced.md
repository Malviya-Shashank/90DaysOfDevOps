# Day 34 – Docker Compose: Real-World Multi-Container Applications

## Objective

Today's goal was to build a **production-like multi-container application using Docker Compose**.

Docker Compose allows defining and managing multiple containers in a single configuration file.  
In this project, a **3-service application stack** was created consisting of:

- Web Application (Flask)
- Database (PostgreSQL)
- Cache (Redis)

The web application connects to both PostgreSQL and Redis to demonstrate container communication inside a Docker network.

---

# Project Architecture

```
Flask Web App
│
├── PostgreSQL Database
│
└── Redis Cache
```

All services run inside the same Docker Compose network and communicate using service names.

---

# Project Structure

```
2026/day-34/

app/
├── app.py
├── requirement.txt
└── Dockerfile

docker-compose.yml
day-34-compose-advanced.md
```

---

# Task 1 – Build a 3-Service Application Stack

A **Docker Compose stack** was created with three services:

| Service | Purpose |
|-------|--------|
| Web | Flask application |
| DB | PostgreSQL database |
| Cache | Redis visit counter |

The Flask application connects to Redis to track visits and PostgreSQL to check database connectivity.

---

# Flask Application Code

## app.py

```python
from flask import Flask
import psycopg2
import redis
import os

app = Flask(__name__)

db_host = os.getenv("DB_HOST", "db")
cache_host = os.getenv("CACHE_HOST", "redis")

def get_db_connection():
    conn = psycopg2.connect(
        host=db_host,
        database="postgres",
        user="postgres",
        password="postgres"
    )
    return conn

cache = redis.Redis(host=cache_host, port=6379)

@app.route("/")
def hello():

    visits = cache.incr("counter")

    try:
        conn = get_db_connection()
        cur = conn.cursor()
        cur.execute("SELECT version();")
        db_version = cur.fetchone()
        cur.close()
        conn.close()
    except Exception:
        db_version = "Database connection failed"

    return f"""
    Hello from Flask DevOps App 🚀 <br>
    Redis Visits Counter: {visits} <br>
    PostgreSQL Version: {db_version}
    """

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

# Requirements File

## requirement.txt

```
flask
psycopg2-binary
redis
```

---

# Dockerfile for Web Application

```
FROM python:3.10-slim

WORKDIR /app

COPY requirement.txt .

RUN pip install --no-cache-dir -r requirement.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```

This Dockerfile builds a custom image for the Flask application.

---

# Task 2 – depends_on & Healthchecks

The web service depends on the database and cache services.

```
depends_on:
  - db
  - redis
```

This ensures that the web container starts **after the database and Redis containers start**.

In production environments, healthchecks are often added so the application waits until the database is fully ready.

---

# Task 3 – Restart Policies

Restart policies help ensure containers restart automatically when failures occur.

Example:

```
restart: always
```

### Restart Policy Comparison

| Policy | Behavior |
|------|------|
| no | Container will not restart |
| on-failure | Restarts only if the container crashes |
| always | Container restarts automatically |

### Use Cases

- **always** → critical services like databases
- **on-failure** → applications that may crash
- **no** → development environments

---

# Task 4 – Custom Dockerfile in Compose

Instead of using a prebuilt image, the Flask application is built using a Dockerfile.

```
web:
  build: ./app
```

To rebuild containers when code changes:

```
docker compose up --build
```

---

# Task 5 – Named Volumes

Named volumes are used to persist database data.

```
volumes:
  postgres_data:
```

Used by PostgreSQL:

```
- postgres_data:/var/lib/postgresql/data
```

This ensures the database data remains safe even if containers are removed.

---

# Docker Compose Configuration

## docker-compose.yml

```
services:

  web:
    build: ./app
    ports:
      - 5000:5000
    environment:
      DB_HOST: db
      CACHE_HOST: redis
    depends_on:
      - db
      - redis

  db:
    image: postgres:15
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: flaskdb
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7

volumes:
  postgres_data:
```

---

# Running the Application

Start all services:

```
docker compose up --build
```

Run in background:

```
docker compose up -d
```

Stop containers:

```
docker compose down
```

Check running containers:

```
docker ps
```

View logs:

```
docker compose logs
```

---

# Testing the Application

Open the browser:

```
http://localhost:5000
```

Example output:

```
Hello from Flask DevOps App 🚀
Redis Visits Counter: 1
PostgreSQL Version: PostgreSQL 15...
```

Every page refresh increases the Redis visit counter.

---

# Task 6 – Scaling (Bonus)

Scaling can be tested using:

```
docker compose up --scale web=3
```

### What Happens

Scaling works, but **port mapping causes conflicts**.

Example:

```
5000:5000
```

Multiple containers cannot bind to the same host port.

### Production Solution

Production systems usually use:

- Load balancers
- Reverse proxies like NGINX
- Kubernetes services

---

# Key Learnings

From this task:

- Docker Compose simplifies multi-container applications
- Containers communicate using service names
- Flask can connect to PostgreSQL and Redis inside Docker networks
- Volumes allow persistent database storage
- Dockerfiles allow building custom application images
- Scaling services may require load balancing

---

# Tags

`#90DaysOfDevOps` `#DevOpsKaJosh` `#TrainWithShubham`
