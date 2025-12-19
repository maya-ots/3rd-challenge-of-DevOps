# Docker Compose Setup
## 3rd-challenge-of-DevOps
Challenge : Create a Docker Compose setup for your application with 3 instances, and configure NGINX as a load balancer using an upstream block. 
(i gotta work on this)
Docker Compose lets you define and run multiple containers using a single file (docker-compose.yml) and one command.
what i need to review : configuring Nginx  as a load balancer using an upstream bloc


## STEPS : 
1️⃣ Prepare your application
Your app must:
Listen on a single internal port (example: 3000)
Not hardcode host/port values

2️⃣ Create a Dockerfile for the app
This image will be reused for all 3 instances.

    Example (Node.js):
    ```node.js
    FROM node:20-alpine

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .

EXPOSE 3000
CMD ["npm", "start"]
```
