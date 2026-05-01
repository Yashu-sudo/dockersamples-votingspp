# 🗳️ Docker Voting App (From Docker Run → Docker Compose)

## 📌 What this project is about

This project is a simple multi-container application that simulates a voting system.

It has:
- A voting app where users vote
- A Redis queue to store votes temporarily
- A worker service that processes votes
- A PostgreSQL database to store results
- A result app to display outcomes

---

## 🤔 Why I built this

I wanted to understand how real-world applications run as multiple services instead of a single app.

This project helped me learn:
- How containers communicate
- How to manage multiple services
- How to debug issues when services depend on each other

---

## 🐳 Phase 1: Using Docker (docker run)

Initially, I ran each container manually using docker run.

Example:
bash docker run -d --name redis redis docker run -d --name db -e POSTGRES_PASSWORD=postgres postgres:15-alpine 

### Problems I faced:
- Managing multiple commands was messy
- Networking had to be handled manually
- Container naming issues caused connection failures
- Hard to restart or reproduce setup

👉 This approach worked, but wasn’t scalable or clean

---

## ⚙️ Phase 2: Moving to Docker Compose

To solve the above problems, I moved to Docker Compose

With Compose:
- All services are defined in one file
- Networking is automatic
- Services communicate using service names (redis, db)
- Easy to start/stop everything with one command

Run everything:
bash docker compose up -d 

---

## 🧠 Key Concept I Learned

> In Docker Compose:
> Service name = hostname

Example:
python redis_host = "redis" db_host = "db" 

No need for IP addresses or linking containers.

---

## 🌐 Access the application

- Voting App → http://<EC2-IP>:1717  
- Result App → http://<EC2-IP>:1818  

---

## 📂 Project Structure

text . ├── docker-compose.yml ├── README.md └── notes/     └── docker-run-commands.md 

---

## 💡 Key Learnings

- Difference between docker run and Docker Compose  
- Container networking and service discovery  
- Debugging container failures (logs, exit issues)  
- Importance of clean orchestration  

---

## 🚀 What I would improve next

- Add health checks for services  
- Add restart policies  
- Move this setup to Kubernetes  

---

## 🧑‍💻 Final Thoughts

Starting with docker run helped me understand the basics,  
but moving to Docker Compose made the setup cleaner, scalable, and closer to real-world usage.

This project helped me connect theory with practical DevOps workflo
