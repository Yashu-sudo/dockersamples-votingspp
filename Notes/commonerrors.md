# ⚠️ Common Errors & Fixes (Docker Voting App)

This file documents common issues faced while working with Docker and Docker Compose, along with their solutions.

---

## ❌ 1. Container exits immediately (PostgreSQL)

### Problem:
Postgres container stops with:
text Exited (1) 

### Cause:
Missing required environment variables.

### Fix:
bash -e POSTGRES_PASSWORD=postgres 

Or in Docker Compose:
yaml environment:   POSTGRES_PASSWORD: postgres 

---

## ❌ 2. Name or service not known

### Problem:
Worker fails with:
text Name or service not known 

### Cause:
Wrong hostname or containers not in same network.

### Fix:
- Use correct service names:
text redis db 
- Ensure all containers are in same network (Compose does this automatically)

---

## ❌ 3. Cannot link to a non-running container

### Problem:
text Cannot link to a non running container 

### Cause:
Trying to connect to a stopped container.

### Fix:
bash docker ps 
Ensure all required containers are running.

---

## ❌ 4. Ports not working

### Problem:
Application not accessible from browser.

### Cause:
Wrong port mapping.

### Fix:
yaml ports:   - "1717:80" 

Access using:
text http://<EC2-IP>:1717 

---

## ❌ 5. YAML indentation errors

### Problem:
Docker Compose fails to run.

### Cause:
Incorrect spacing or missing - in lists.

### Fix:
yaml ports:   - "5432:5432" 

Always maintain proper indentation.

---

## ❌ 6. Image not found

### Problem:
text pull access denied 

### Cause:
Image name incorrect or not built.

### Fix:
bash docker build -t votingimg ./vote 

---

## ❌ 7. Worker crashes even when DB is running

### Cause:
Database not ready yet (initializing).

### Fix:
- Restart worker:
bash docker restart worker 
- Or use retry logic in application

---

## ❌ 8. Confusion between container name and service name

### Cause:
Using wrong name for communication.

### Fix:
Use service name:
text redis db 

---

## ❌ 9. Docker Compose command not found

### Problem:
text docker: 'compose' is not a docker command 

### Fix:
bash sudo dnf install docker-compose-plugin -y 

---

## 🧠 Key Learnings

- Always check logs:
bash docker logs <container> 

- Verify running containers:
bash docker ps 

- Understand networking and service names

---

## 🎯 Summary

Most Docker issues are related to:
- Networking
- Environment variables
- YAML syntax
- Service readiness

Understanding these helps debug quickly and efficiently.
