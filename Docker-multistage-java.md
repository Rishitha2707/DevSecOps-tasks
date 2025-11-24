**🚀 Java WAR Application – CI-Ready Multi-Stage Docker Build**
This project demonstrates how to build and deploy a Java web application using a single multi-stage Dockerfile. The workflow includes:

✔ Clone Java source code from GitHub
✔ Build the WAR using Maven
✔ Deploy the generated WAR into Apache Tomcat
✔ Run the application inside a fully containerized environment

This setup is ideal for learning DevOps pipelines or running lightweight Java web deployments.

--------------------------------------------------------------------------------------------------------------------------------------------

**📁 Project Used**

GitHub Repository: https://github.com/Rishitha2707/war-web-project.git

The repository contains a simple Java web application packaged as a WAR file using Maven.

------------------------------------------------------------------------------------------------------------------------------------------

**🧩 Multi-Stage Docker Architecture**
This project uses three Docker stages:
1. Clone Stage – Pull source code from GitHub
2. Build Stage – Use Maven to package the WAR
3. Deploy Stage – Copy the WAR file into Tomcat and run the server

This makes the final image:
. Much smaller
. More secure
. More production-friendly

<img width="1421" height="355" alt="Screenshot 2025-11-20 104042" src="https://github.com/user-attachments/assets/8a87885d-96e3-4a87-8f13-cbf2323b2a4c" />

--------------------------------------------------------------------------------------------------------------------------------------------

**🐳 Multi-Stage Dockerfile (Clone → Build → Deploy)**

```
##############################
# Stage 1: Clone Source Code
##############################
FROM alpine/git AS clone

WORKDIR /app

# Clone your repository
RUN git clone https://github.com/Rishitha2707/war-web-project.git .

##############################
# Stage 2: Build using Maven
##############################
FROM maven:3.9.5-eclipse-temurin-17 AS build

WORKDIR /app

# Copy cloned code from previous stage
COPY --from=clone /app /app

# Build WAR file
RUN mvn clean package -DskipTests

##############################
# Stage 3: Run with Tomcat
##############################
FROM tomcat:9.0-jdk17 AS final

# Remove default ROOT app
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy WAR from Maven build stage
COPY --from=build /app/target/*.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080

CMD ["catalina.sh", "run"]
```

-----------------------------------------------------------------------------------------------------------------------------------------


**⚙️ Step-by-Step Explanation**

1️⃣ Clone Stage
Uses an extremely lightweight alpine/git image to fetch the latest source code automatically:
```
FROM alpine/git AS clone
RUN git clone <repo-url> .
```
This removes the need to manually copy project files.

2️⃣ Build Stage (Maven)
The project is built without installing Maven locally. Docker takes care of everything.
```
FROM maven:3.9.5-eclipse-temurin-17 AS build
RUN mvn clean package -DskipTests
```
This produces a WAR file inside:
```
target/*.war
```

3️⃣ Deploy Stage (Tomcat)
A fresh Tomcat server is used. The built WAR is copied into the webapps directory:
```
FROM tomcat:9.0-jdk17
COPY --from=build /app/target/*.war /usr/local/tomcat/webapps/ROOT.war
```
When the container starts, Tomcat automatically deploys the WAR.

------------------------------------------------------------------------------------------------------------------------------------------

**🧪 Building the Docker Image**

Run the following in your terminal:
```
docker build -t java-war-app .
```

This will:
1. Clone the code
2. Build the WAR
3. Deploy into Tomcat
4. Output a final runnable image

<img width="1457" height="369" alt="Screenshot 2025-11-21 143219" src="https://github.com/user-attachments/assets/219fbf83-3bc8-4605-9b99-58e1ac386797" />

<img width="1428" height="418" alt="Screenshot 2025-11-21 143229" src="https://github.com/user-attachments/assets/13c5abc6-8dbd-41d4-805a-bccf78e6d001" />


<img width="1475" height="515" alt="Screenshot 2025-11-21 143209" src="https://github.com/user-attachments/assets/d5cdf583-6688-4f82-bbf2-b9f5a509f19a" />


--------------------------------------------------------------------------------------------------------------------------------------------

**▶️ Running the Container**

```
docker run -p 8080:8080 java-war-app
```
Now access the application at:
```
http://localhost:8080
```
<img width="1907" height="1012" alt="Screenshot 2025-11-21 143201" src="https://github.com/user-attachments/assets/727aa533-9bdc-49f6-b2f6-cb86f34629bf" />

------------------------------------------------------------------------------------------------------------------------------------------

📦 Folder Structure (Explained)
```
Dockerfile
/app (created dynamically inside container)
  └── war-web-project
      ├── src/
      ├── pom.xml
      └── target/*.war (after build)
```

Everything stays isolated — nothing touches your local system.


**🎯 Why Multi-Stage Docker?**

| Feature       | Benefit                                                  |
| ------------- | -------------------------------------------------------- |
| Smaller Image | Final Tomcat image contains only runtime files           |
| Secure        | Build tools (Maven, Git) are not in the production image |
| Repeatable    | Every build fetches fresh code                           |
| CI/CD Ready   | Works perfectly with Jenkins, GitHub Actions, GitLab CI  |











