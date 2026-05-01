# ⚙️ Docker Compose Explained (Voting App)

This file explains how the docker-compose.yml is structured and how each service works together.

---

## 🧱 What is Docker Compose?

Docker Compose is used to run multiple containers using a single configuration file.

Instead of running multiple docker run commands, we define everything in one file and start it with:

bash docker compose up -d 

---

## 📂 Services Overview

In Compose, everything runs under:

yaml services: 

Each service represents a container.

---

## 🔴 Redis Service

yaml redis:   image: redis 

- Acts as a message queue
- Stores votes temporarily
- Other services access it using hostname: redis

---

## 🗄️ Database (PostgreSQL)

yaml db:   image: postgres:15-alpine   environment:     POSTGRES_USER: postgres     POSTGRES_PASSWORD: postgres     POSTGRES_DB: postgres   ports:     - "5432:5432" 

- Stores final voting results
- Environment variables initialize the database
- Accessible inside network as db

---

## 🗳️ Vote Service

yaml vote:   image: votingimg   ports:     - "1717:80"   depends_on:     - redis 

- Frontend application for users to vote
- Exposed on port 1717
- Depends on Redis to send vote data

---

## ⚙️ Worker Service

yaml worker:   image: workercont   depends_on:     - redis     - db 

- Background service
- Reads votes from Redis
- Writes processed data into PostgreSQL

---

## 📊 Result Service

yaml result:   image: resultimage   ports:     - "1818:80"   depends_on:     - db 

- Displays results from database
- Accessible via port 1818

---

## 🧠 Important Concepts

### 🔹 Service Names = Hostnames

Containers communicate using service names:

text redis → Redis container db → PostgreSQL container 

No need for IP addresses.

---

### 🔹 depends_on

Used to define startup order:

yaml depends_on:   - redis 

⚠️ Note: It ensures container starts, but not that service is ready.

---

### 🔹 Ports

yaml ports:   - "1717:80" 

- Left side → host port
- Right side → container port

---

## 🔗 How Everything Connects

text User → Vote App → Redis → Worker → PostgreSQL → Result App 

---

## 🚀 Why Docker Compose is better

Compared to docker run:

- One file instead of many commands
- Automatic networking
- Easier to manage and restart
- Clean and scalable setup

---

## 🎯 Summary

Docker Compose simplifies multi-container applications by:
- Defining all services in one place
- Managing networking automatically
- Enabling service discovery via n
