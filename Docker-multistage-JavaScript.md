**📘 Portfolio Website – Dockerized Multi-Stage Build & Deployment**

This project demonstrates a complete multistage Docker pipeline for a JavaScript/Node.js static website.

The workflow includes:
1. Cloning the source code from GitHub
2. Building the JavaScript project with Node.js & npm
3. Deploying the production build using Nginx

This produces a single optimized Docker image suitable for production hosting.

-----------------------------------------------------------------------------------------------------------------------------------------------------

**🏗️ Project Overview**

This repository contains the source code for a portfolio website built using JavaScript (React or standard JS).
To optimize performance and make deployment lightweight, we use a multistage Docker build.

-----------------------------------------------------------------------------------------------------------------------------------------------------

**🚀 Multi-Stage Docker Workflow**

Stage 1 → Clone Code from GitHub
We use alpine/git to fetch the project from GitHub:
```
FROM alpine/git AS clone
WORKDIR /app
RUN git clone https://github.com/Rishitha2707/Portfolio-Website.git .
```
✔️ Light-weight git client

✔️ Keeps the final image clean (code is not cloned inside Nginx)


Stage 2 → Build with Node.js
We build the project using a Node.js environment:
```
FROM node:18-alpine AS build
WORKDIR /app
COPY --from=clone /app /app
RUN npm install
RUN npm run build
```

✔️ Installs dependencies

✔️ Runs the production build

✔️ Generates optimized static files in build/


Stage 3 → Deploy with Nginx
Nginx is used for serving the built static files:
```
FROM nginx:alpine AS final
RUN rm -rf /usr/share/nginx/html/*
COPY --from=build /app/build/ /usr/share/nginx/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
✔️ Fast & lightweight

✔️ Production-ready static hosting

✔️ Only Nginx + build output in the final image

-------------------------------------------------------------------------------------------------------------------------------------

**📦 Final Dockerfile**

```
##############################
# Stage 1: Clone GitHub Repo
##############################
FROM alpine/git AS clone

WORKDIR /app

RUN git clone https://github.com/Rishitha2707/Portfolio-Website.git .

##############################
# Stage 2: Build with Node.js
##############################
FROM node:18-alpine AS build

WORKDIR /app

COPY --from=clone /app /app

RUN npm install

RUN npm run build     # Output in /app/build/

##############################
# Stage 3: Deploy using Nginx
##############################
FROM nginx:alpine AS final

RUN rm -rf /usr/share/nginx/html/*

# Copy the React/JS build output
COPY --from=build /app/build/ /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```
---------------------------------------------------------------------------------------------------------------------------------------------

**🛠️ How to Build the Docker Image**

Run the following command:
```
docker build -t portfolio-site .
```

<img width="1252" height="602" alt="Screenshot 2025-11-21 144310" src="https://github.com/user-attachments/assets/4bbf4eaf-37e4-418a-bedd-c0e48e3100ea" />


<img width="1410" height="681" alt="Screenshot 2025-11-21 144301" src="https://github.com/user-attachments/assets/909e9e7e-3565-4f7b-94de-65348478656f" />




---------------------------------------------------------------------------------------------------------------------------------------------

**▶️ How to Run the Container**

```
docker run -p 80:80 portfolio-site
```

Now open your browser:
```
http://localhost
```

<img width="1919" height="961" alt="Screenshot 2025-11-21 144250" src="https://github.com/user-attachments/assets/f110ef24-ec2a-43a7-bf32-9ffad7f59fab" />


---------------------------------------------------------------------------------------------------------------------------------------------

**📁 Folder Structure**
```
Portfolio-Website/
│── public/
│── src/
│── package.json
│── Dockerfile        ← Multi-stage Dockerfile
│── README.md         ← This documentation
```
-----------------------------------------------------------------------------------------------------------------------------------------------

**⭐ Benefits of This Approach**

| Feature             | Benefit                                      |
| ------------------- | -------------------------------------------- |
| Multi-Stage Docker  | Small, optimized production image            |
| Git Clone Stage     | Clean separation of build & fetch operations |
| Node.js Build Stage | Proper dependency installation               |
| Nginx Deployment    | Fast & secure static hosting                 |
| Single Dockerfile   | CI/CD & cloud ready                          |


