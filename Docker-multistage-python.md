**📌 ATM Management App — Dockerized Flask Application**

This project contains a simple ATM Web Application built using Python + Flask, fully automated using Docker multi-stage build (clone → build → deploy).

The Dockerfile automatically:

1. Clones the GitHub repository
2. Installs dependencies
3. Runs Flask app inside a container

<img width="1421" height="355" alt="Screenshot 2025-11-20 104042" src="https://github.com/user-attachments/assets/01050595-6840-4580-abba-1939f1c9c91b" />


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

<img width="874" height="674" alt="Screenshot 2025-11-21 170519" src="https://github.com/user-attachments/assets/4ab5e2c3-c87d-4b6c-8437-93969450ae8d" />


**▶️ How to Run the Container**
Run Python Flask app:
```
docker run -d --name atm-app -p 5000:5000 python-atm-app
```

<img width="1431" height="164" alt="Screenshot 2025-11-24 112916" src="https://github.com/user-attachments/assets/8160bad1-e825-4c93-bc4b-814955660919" />


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

**OutPut**

<img width="1917" height="955" alt="Screenshot 2025-11-24 110217" src="https://github.com/user-attachments/assets/fe0a8a0a-df6d-4bbc-93f9-23e11a5504a9" />


<img width="1918" height="539" alt="Screenshot 2025-11-24 110247" src="https://github.com/user-attachments/assets/f3bb9e90-0a5d-4d26-9c32-76f897ee63f0" />
