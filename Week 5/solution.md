# 🚀 Week 5: Docker Basics & Advanced Challenge – Solutions

This file contains solutions and commands for all tasks from **Week 5 of #90DaysOfDevOps – Docker Basics & Advanced Challenge**.

---

## ✅ Task 1: Docker Installation
Install Docker on your system and verify installation.

```bash
docker --version
docker info
````

---

## ✅ Task 2: Running Your First Container

Run a simple container (`hello-world`) and list containers.

```bash
docker run hello-world
docker ps -a
```

---

## ✅ Task 3: Dockerfile Creation

Create a Dockerfile for a simple Node.js app.

**Dockerfile**

```dockerfile
FROM node:16
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
CMD ["node", "app.js"]
```

Build and run:

```bash
docker build -t my-node-app .
docker run -p 3000:3000 my-node-app
```

---

## ✅ Task 4: Docker Compose

Create a `docker-compose.yml` to run Node.js app with MongoDB.

**docker-compose.yml**

```yaml
version: '3'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - mongo
  mongo:
    image: mongo
    ports:
      - "27017:27017"
```

Run:

```bash
docker-compose up -d
docker-compose ps
```

---

## ✅ Task 5: Pushing Image to Docker Hub

Push your custom image to Docker Hub.

```bash
docker login
docker tag my-node-app <your-dockerhub-username>/my-node-app:v1
docker push <your-dockerhub-username>/my-node-app:v1
```

---

## ✅ Task 6: Docker Volumes

Use a Docker volume for persistent storage.

```bash
docker volume create mydata
docker run -d -v mydata:/app/data --name myapp my-node-app
docker volume ls
docker inspect mydata
```

---

## ✅ Task 7: Docker Networking

Create a custom Docker network and connect containers.

```bash
docker network create mynetwork
docker run -d --name mongo --network mynetwork mongo
docker run -d -p 3000:3000 --name app --network mynetwork my-node-app
docker network ls
docker network inspect mynetwork
```

---

## ✅ Task 8: Docker Scout (Image Analysis)

Analyze image vulnerabilities using Docker Scout.

```bash
docker scout quickview my-node-app
docker scout cves my-node-app
```

---
