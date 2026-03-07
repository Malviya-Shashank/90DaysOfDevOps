# Day 32 – Docker Volumes & Networking

## Overview
Today’s focus was on solving two real-world Docker problems:

1. **Data persistence** – Containers lose data when removed.
2. **Container communication** – Containers need networking to talk to each other.

Using **Docker Volumes** and **Docker Networks**, we can ensure persistent data storage and allow containers to communicate reliably.

---

# Task 1 – The Problem (Container Data Loss)

### Run a MySQL Container

```bash
docker run -d \
--name mysql-test \
-e MYSQL_ROOT_PASSWORD=rootpass \
mysql:8
```

### Access MySQL

```bash
docker exec -it mysql-test mysql -uroot -p
```

Password:

```
rootpass
```

### Create Data

```sql
CREATE DATABASE devops;

USE devops;

CREATE TABLE users (
id INT PRIMARY KEY,
name VARCHAR(50)
);

INSERT INTO users VALUES (1,'DevOpsUser');

SELECT * FROM users;
```

### Remove Container

```bash
docker stop mysql-test
docker rm mysql-test
```

### Run Container Again

```bash
docker run -d \
--name mysql-test \
-e MYSQL_ROOT_PASSWORD=rootpass \
mysql:8
```

### Result

The database and tables are **missing**.

### Explanation

Containers store data **inside their filesystem**.  
When the container is deleted, the filesystem is deleted as well.

This is why **persistent storage is needed**.

---

# Task 2 – Named Volumes

Named volumes store container data **outside the container lifecycle**.

### Create a Volume

```bash
docker volume create mysql-data
```

### Run MySQL Using Volume

```bash
docker run -d \
--name mysql-volume \
-e MYSQL_ROOT_PASSWORD=rootpass \
-v mysql-data:/var/lib/mysql \
mysql:8
```

### Insert Data Again

```bash
docker exec -it mysql-volume mysql -uroot -p
```

Add some records again.

### Remove Container

```bash
docker stop mysql-volume
docker rm mysql-volume
```

### Run New Container With Same Volume

```bash
docker run -d \
--name mysql-volume-new \
-e MYSQL_ROOT_PASSWORD=rootpass \
-v mysql-data:/var/lib/mysql \
mysql:8
```

### Verify

```bash
docker volume ls
```

```bash
docker volume inspect mysql-data
```

### Result

The **data is still present**.

### Explanation

Docker volumes persist data even when containers are deleted.

---

# Task 3 – Bind Mounts

Bind mounts link **a host directory directly to a container directory**.

### Create Folder

```bash
mkdir my-website
cd my-website
```

### Create HTML File

Create a file called `index.html`

```html
<h1>Hello from Docker Bind Mount</h1>
```

### Run Nginx Container

```bash
docker run -d \
--name nginx-bind \
-p 8080:80 \
-v $(pwd):/usr/share/nginx/html \
nginx
```

### Open Browser

```
http://localhost:8080
```

### Modify File

Edit `index.html`:

```html
<h1>Updated from Host Machine</h1>
```

Refresh browser.

### Result

Changes appear **immediately**.

### Difference

| Feature | Named Volume | Bind Mount |
|-------|-------|-------|
| Managed by Docker | Yes | No |
| Good for databases | Yes | Not recommended |
| Good for development | Sometimes | Yes |
| Host directory control | Limited | Full |

---

# Task 4 – Docker Networking Basics

### List Networks

```bash
docker network ls
```

Default networks include:

```
bridge
host
none
```

### Inspect Bridge Network

```bash
docker network inspect bridge
```

### Run Two Containers

```bash
docker run -dit --name container1 alpine sh
docker run -dit --name container2 alpine sh
```

### Ping by Container Name

```bash
docker exec container1 ping container2
```

Result:

```
Ping failed
```

### Ping by IP Address

Get container IP:

```bash
docker inspect container2
```

Then:

```bash
docker exec container1 ping <container2-ip>
```

Result:

```
Ping successful
```

---

# Task 5 – Custom Docker Networks

Custom networks provide **built-in DNS resolution**.

### Create Network

```bash
docker network create my-app-net
```

### Run Containers on Network

```bash
docker run -dit --name app1 --network my-app-net alpine sh
docker run -dit --name app2 --network my-app-net alpine sh
```

### Test Communication

```bash
docker exec app1 ping app2
```

Result:

```
Ping successful
```

### Explanation

Custom networks allow containers to communicate using **container names instead of IP addresses**.

---

# Task 6 – Full Example (App + Database)

### Create Network

```bash
docker network create app-network
```

### Create Database Volume

```bash
docker volume create app-db-data
```

### Run Database Container

```bash
docker run -d \
--name app-db \
--network app-network \
-e MYSQL_ROOT_PASSWORD=rootpass \
-v app-db-data:/var/lib/mysql \
mysql:8
```

### Run App Container

```bash
docker run -dit \
--name app-container \
--network app-network \
alpine sh
```

### Test Connection

```bash
docker exec app-container ping app-db
```

Result:

```
Successful communication
```

The app container can reach the database using the **container name**.

---

# Key Learnings

### Containers Are Ephemeral
Containers lose data when removed unless persistent storage is used.

### Docker Volumes Provide Persistence
Named volumes allow data to survive container deletion.

### Bind Mounts Sync Host and Container Files
Bind mounts allow live file editing from the host.

### Docker Networking Enables Container Communication
Containers communicate via:

- **IP addresses on default networks**
- **Container names on custom networks**

---

# Screenshots

Add screenshots for:

- `docker volume ls`
- Data persistence test
- Nginx bind mount page
- `docker network ls`
- Container ping tests

---

# Conclusion

Today’s experiments demonstrated how Docker handles **data persistence and container networking**.

Using volumes ensures that critical data is not lost, while custom networks allow containers to communicate efficiently in multi-container environments.

These concepts are fundamental when running **real production applications with Docker**.