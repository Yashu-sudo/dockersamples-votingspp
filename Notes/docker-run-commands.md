# 🐳 Running the Voting App using Docker Run (Step by Step)

This section documents how the application was initially run using individual docker run commands before moving to Docker Compose.

---

## 🔹 Step 1: Create a Network

All containers must be able to communicate, so we create a custom network:

bash docker network create voting-app 

---

## 🔹 Step 2: Run Redis

Redis acts as a message queue between the voting app and worker.

bash docker run -d \ --name redis \ --network voting-app \ redis 

---

## 🔹 Step 3: Run PostgreSQL (Database)

PostgreSQL stores the processed voting results.

bash docker run -d \ --name db \ --network voting-app \ -e POSTGRES_USER=postgres \ -e POSTGRES_PASSWORD=postgres \ -e POSTGRES_DB=postgres \ postgres:15-alpine 

---

## 🔹 Step 4: Run Voting App

Frontend where users cast their vote.

bash docker run -d \ --name vote \ --network voting-app \ -p 1717:80 \ votingimg 

---

## 🔹 Step 5: Run Worker

Worker consumes data from Redis and writes to PostgreSQL.

bash docker run -d \ --name worker \ --network voting-app \ workercont 

---

## 🔹 Step 6: Run Result App

Displays the voting results stored in PostgreSQL.

bash docker run -d \ --name result \ --network voting-app \ -p 1818:80 \ resultimage 

---

## 🧠 Key Observations

- Containers communicate using container names (redis, db)
- Manual setup requires careful ordering and configuration
- Debugging requires checking logs individually

---

## ⚠️ Limitations of this approach

- Multiple commands to manage
- Hard to maintain and scale
- Networking and dependencies handled manually

---

## 🚀 Why moved to Docker Compose

To simplify:
- One file for all services
- Automatic networking
- Service discovery using service names
- Easier startup and management

bash docker compose up -d
