**📌 ATM Management App — Dockerized Flask Application**

This project contains a simple ATM Web Application built using Python + Flask, fully automated using Docker multi-stage build (clone → build → deploy).

The Dockerfile automatically:

1. Clones the GitHub repository
2. Installs dependencies
3. Runs Flask app inside a container


**🐳 Dockerfile (Multi-Stage Build: clone → build → run)**

```
# ------------------------ #
# STAGE 1 → Clone Repo
# ------------------------ #
FROM alpine/git AS clone
WORKDIR /src
RUN git clone https://github.com/Rishitha2707/sample.git .

# ------------------------ #
# STAGE 2 → Build Python App
# ------------------------ #
FROM python:3.10-slim AS build
WORKDIR /app

# Copy the mini_project folder
COPY --from=clone /src/mini_project /app/mini_project

# Install Flask and dependencies
RUN pip install --no-cache-dir flask

# ------------------------ #
# STAGE 3 → Final Runtime Image
# ------------------------ #
FROM python:3.10-slim
WORKDIR /app

# Copy built application
COPY --from=build /app /app

EXPOSE 5000

# Run Flask app
CMD ["python", "mini_project/ATM.py"]
```

**📦 How to Build the Docker Image**
Run the following from your EC2 or local machine:
```
docker build -t python-atm-app .
```

**▶️ How to Run the Container**
Run Python Flask app:
```
docker run -d --name atm-app -p 5000:5000 python-atm-app
```

**🌐 Access the Application**
From browser:
```
http://<EC2-PUBLIC-IP>:5000
```

**⚙️ Flask APP (ATM.py) — Overview**
Features implemented:

✔ User Login

✔ View Balance

✔ Deposit Amount

✔ Withdraw Amount

✔ Logout

✔ Session Management

Sample User Accounts:
| Username | PIN  | Balance |
| -------- | ---- | ------- |
| nikki    | 1234 | 5000    |
| admin    | 0000 | 10000   |


**🎯 Tech Stack**

| Component       | Description                     |
| --------------- | ------------------------------- |
| **Python 3.10** | Backend runtime                 |
| **Flask**       | Web framework                   |
| **Docker**      | App containerization            |
| **Alpine Git**  | For cloning repo in first stage |


**📁 Folder Layout After Clone**

```
sample/
 └── mini_project/
        ├── ATM.py
        └── templates/
            ├── login.html
            ├── menu.html
            ├── balance.html
            ├── deposit.html
            └── withdraw.html
```

