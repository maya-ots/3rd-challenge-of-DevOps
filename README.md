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
    FROM node:20-alpine
    WORKDIR /app
    COPY package*.json ./
    RUN npm install
    COPY . .
    EXPOSE 3000
    CMD ["npm", "start"]
Builds one image, runs multiple containers.

3️⃣ Create the NGINX configuration (load balancer)
Create a folder: 

    nginx/
Inside it, create nginx.conf:

    events {}

    http {
    upstream app_backend {
        server app1:3000;
        server app2:3000;
        server app3:3000;
     }

    server {
        listen 80;

        location / {
            proxy_pass http://app_backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
      }
    }

🔹 upstream block defines the 3 app instances
🔹 NGINX automatically load-balances (round-robin by default)

4️⃣ Create docker-compose.yml
This is the core of the challenge.

       version: "3.9"

    services:
    app1:
    build: .
    container_name: app1
    expose:
      - "3000"

    app2:
    build: .
    container_name: app2
    expose:
      - "3000"

    app3:
    build: .
    container_name: app3
    expose:
      - "3000"

    nginx:
    image: nginx:alpine
    container_name: nginx-lb
    ports:
      - "80:80"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - app1
      - app2
      - app3

Key points:
expose → internal communication only
ports → only NGINX is exposed to the host
All services share the same Docker network automatically

Build and start everything
Run:

    docker compose up --build

