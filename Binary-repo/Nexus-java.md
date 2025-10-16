**🚀 DevOps Project: Continuous Delivery Pipeline using Nexus Repository & Maven for Java Web Application
**
This project demonstrates the implementation of a Continuous Delivery (CD) pipeline using Sonatype Nexus Repository Manager 3, Apache Maven, and Apache Tomcat across two separate servers.

The project automates the build, versioning, and deployment process of a Java Web Calculator Application, following real-world DevOps practices.

🏗️ Project Architecture:
```
+-----------------------------+          +-----------------------------+
|         Nexus Server        |          |     Build & Deploy Server   |
|     (Artifact Repository)   |          |       (Maven + Tomcat)      |
|                             |          |                             |
| - Nexus 3.79.0-09           | <---- Push ---- | - Builds WAR using Maven |
| - Stores WAR Artifacts      | ---- Pull ----> | - Deploys to Tomcat      |
+-----------------------------+          +-----------------------------+
```


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
<img width="1080" height="88" alt="Image" src="https://github.com/user-attachments/assets/50d5c217-e54c-4ce2-bd46-c44cfb4cb4d7" />

```
# 2. Download Nexus 3 tarball
wget https://download.sonatype.com/nexus/3/nexus-unix-x86-64-3.79.0-09.tar.gz
```
<img width="1456" height="379" alt="Image" src="https://github.com/user-attachments/assets/acaa626c-6004-4f6b-b269-c5524850d872" />

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
      <img width="964" height="69" alt="Image" src="https://github.com/user-attachments/assets/9c769970-7188-486f-9815-82713b7b860a" />

    3. Set a new password (e.g., nexus).
    

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/8288d2cd-2861-401e-b92e-d427902c3363" />

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/72278816-3ac6-4e94-a335-f248c9611c3d" />

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/bf27e6db-7f95-4fe0-a461-fd6283e13d5e" />

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/3805b9b0-5342-4a9b-9800-54e06cd3c200" />


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


<img width="1100" height="264" alt="Image" src="https://github.com/user-attachments/assets/dd245041-9735-4025-bad5-f4e9ffaea1ff" />


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
```
vi pom.xml
```
add this:(to tell maven that the artifact has to be deployed intot his particular repository)
<img width="918" height="171" alt="Image" src="https://github.com/user-attachments/assets/982122be-3665-4ac6-91df-5570d8824330" />

```
sudo vi /etc/maven/settings.xml
```
add this: for authentication to nexus
<img width="385" height="130" alt="Image" src="https://github.com/user-attachments/assets/1d89e2de-8ebb-4585-9342-3eac0e9f278b" />


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
<img width="1471" height="189" alt="Image" src="https://github.com/user-attachments/assets/7d28645a-9758-4b32-ae32-a50902090d90" />

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/bae003d4-33ef-4419-a568-bbe7ca95211f" />

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/79884d75-4b24-43ef-82a5-50dc3923332f" />

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/509de463-ad94-4f23-841a-ead109575b4a" />

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
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/d43f0b19-b5a4-4cb3-9177-5d7709210a57" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/6034cb5e-e181-4839-9697-e4ab0f3127d7" />


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
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/3b1ebaef-09e6-4975-91e3-dbd6f0e2b579" />

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/d8c0e2e6-20d0-404c-b524-458bc26c48ee" />

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
