🚀 DevOps Project: Continuous Delivery Pipeline using Nexus Repository & Maven for Java Web Application

This project demonstrates the implementation of a Continuous Delivery (CD) pipeline using Sonatype Nexus Repository Manager 3, Apache Maven, and Apache Tomcat across two separate servers.

The project automates the build, versioning, and deployment process of a Java Web Calculator Application, following real-world DevOps practices.

🏗️ Project Architecture:
+-----------------------------+                 +-----------------------------+
|       Nexus Server          |                 |     Build & Deploy Server   |
| (Artifact Repository)       |                 | (Maven + Tomcat)            |
|                             |                 |                             |
|  - Nexus 3.79.0-09          |   <---- Push ---|  - Builds WAR using Maven   |
|  - Stores WAR Artifacts     |   ---- Pull --->|  - Deploys to Tomcat        |
+-----------------------------+                 +-----------------------------+


🖥️ Server 1: Nexus Repository Manager Setup
Purpose

Acts as a centralized artifact repository to store and manage different versions of Java web application builds (WAR files).

| Parameter        | Description                             |
| ---------------- | --------------------------------------- |
| **Server Role**  | Artifact Repository                     |
| **Software**     | Nexus Repository Manager 3 (v3.79.0-09) |
| **Port**         | 8081                                    |
| **IP Address**   | `3.101.111.145`                         |
| **Prerequisite** | Java JRE/JDK must be installed          |


Setup Commands:
```
# 1. Verify Java installation
java --version
```
```
# 2. Download Nexus 3 tarball
wget https://download.sonatype.com/nexus/3/nexus-unix-x86-64-3.79.0-09.tar.gz
```

```
# 3. Extract files
tar -xvf nexus-unix-x86-64-3.79.0-09.tar.gz
```

```
# 4. Navigate to Nexus directory and start service
cd nexus-3.79.0-09/
cd bin/
./nexus start
```

```
# 5. Retrieve initial admin password
cat /home/ubuntu/sonatype-work/nexus3/admin.password
```

Nexus UI Configuration
   1. Access the Nexus Web Interface:
    🔗 http://3.101.111.145:8081

   2. Login using:

        . Username: admin

        . Password: (from the admin.password file)

    3. Set a new password (e.g., nexus).


Repository Used

    . Repository Name: Maven Releases

    . Repository URL: http://3.101.111.145:8081/repository/maven-releases/
    



🧩 Server 2: Build & Deploy Server Setup

Purpose
Handles the building, versioning, and deployment of the Java Web Application using Maven and Tomcat.

| Parameter                    | Description                                                                           |
| ---------------------------- | ------------------------------------------------------------------------------------- |
| **Server Role**              | Build & Deployment                                                                    |
| **Software**                 | Git, Maven, Java, Apache Tomcat 9.0.111                                               |
| **Application Source**       | [Java Web Calculator App](https://github.com/mrtechreddy/Java-Web-Calculator-App.git) |
| **Authentication for Nexus** | `admin:nexus`                                                                         |

⚙️ Step-by-Step Process
1️⃣ Clone the Application
```
git clone https://github.com/mrtechreddy/Java-Web-Calculator-App.git
```

2️⃣ Navigate to Tomcat Webapps Folder
```
cd apache-tomcat-9.0.111/
cd webapps/
```

3️⃣ Iteration 1 – Addition Feature (v0.0.1)
```
# Download the first version from Nexus and deploy to Tomcat
wget http://admin:nexus@3.101.111.145:8081/repository/maven-releases/com/web/cal/webapp-add/0.0.1/webapp-add-0.0.1.war

# Verify deployment
ls
```

🧠 What was done:

Updated pom.xml → version 0.0.1, artifactId webapp-add

Modified index.jsp to include Addition functionality

Built and deployed to Nexus:
```
mvn clean install
mvn deploy
```

4️⃣ Iteration 2 – Addition + Subtraction Feature (v0.0.2)
```
cd Java-Web-Calculator-App/
vi pom.xml
vi src/main/webapp/index.jsp
```

# Build and deploy new version
```
mvn clean install
mvn deploy
```

# Deploy from Nexus to Tomcat
```
cd ../apache-tomcat-9.0.111/webapps/
wget http://admin:nexus@3.101.111.145:8081/repository/maven-releases/com/web/cal/webapp-add-sub/0.0.2/webapp-add-sub-0.0.2.war
ls
```

🧠 What was done:

Added Subtraction logic.

Updated pom.xml → artifactId webapp-add-sub, version 0.0.2



5️⃣ Iteration 3 – Addition + Subtraction + Product Feature (v0.0.3)
```
cd Java-Web-Calculator-App/
vi pom.xml
vi src/main/webapp/index.jsp
```

```
# Build and deploy new version
mvn clean install
mvn deploy
```

# Deploy from Nexus to Tomcat
```
cd ../apache-tomcat-9.0.111/webapps/
wget http://admin:nexus@3.101.111.145:8081/repository/maven-releases/com/web/cal/webapp-add-sub-product/0.0.3/webapp-add-sub-product-0.0.3.war
```

🧠 What was done:

Added Multiplication (Product) feature.

Updated version → 0.0.3, artifactId → webapp-add-sub-product


📂 Final Deployment State

All versions were successfully deployed under Tomcat’s webapps directory:
```
webapp-add-0.0.1/
webapp-add-sub-0.0.2/
webapp-add-sub-product-0.0.3/
```

Each version can be accessed independently, demonstrating version control and artifact traceability.


🧾 Summary of Accomplishments
| Key Area                   | Description                                                                            |
| -------------------------- | -------------------------------------------------------------------------------------- |
| **Artifact Management**    | Nexus configured as centralized repository for all WAR builds.                         |
| **Automated Builds**       | Maven used for consistent build, versioning, and deployment.                           |
| **Version Control**        | Application iteratively evolved through versions 0.0.1 → 0.0.3.                        |
| **Deployment Automation**  | Artifacts fetched directly from Nexus using wget and basic auth.                       |
| **Environment Separation** | Nexus and Build/Deploy run on separate servers, mirroring real-world DevOps pipelines. |



🏁 Final Outcome

✅ A fully functional Continuous Delivery (CD) pipeline integrating:

    Sonatype Nexus 3 for artifact management

    Apache Maven for automated builds and versioning

    Apache Tomcat for deployment

Each new version of the Java Web Calculator app was automatically:

    Built using Maven

    Deployed to Nexus

    Pulled and Deployed on Tomcat

This demonstrates the core DevOps principles of automation, repeatability, and versioned delivery.


🧠 Learnings

Setting up artifact repositories with Nexus.

Automating build and deploy cycles with Maven.

Managing application versioning effectively.

Implementing multi-server architecture for CI/CD pipelines.