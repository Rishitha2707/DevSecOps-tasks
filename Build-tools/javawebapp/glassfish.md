Project: CI/CD Pipeline for Java Web Application with Maven and GlassFish
Synopsis
This project demonstrates a Build Server compiles and packages a Java web application into a WAR file using Apache Maven (with Java 8). The resulting artifact is then securely transferred to a Web Server, where it is deployed and hosted on the GlassFish 5 application server. This process automates the build and deployment stages, showcasing a core DevOps workflow.

Requirements

Build Server:

    Ubuntu 24.04 LTS

    Java 8 (OpenJDK 1.8.0)

    Apache Maven 3.8.7

    Git

Web Server:

    Ubuntu 24.04 LTS

    GlassFish Server 5.1.0

    Java (to run GlassFish)

Network:

    SSH access between servers.

    A PEM key file for secure SCP file transfer.



Implementation Steps


Phase 1: Build Server Setup

1. Installing Java 8
The build server required a specific version of Java. Java 8 was installed using apt.
```
sudo apt update -y
sudo apt install openjdk-8-jre-headless -y
```

2. Installing Maven
```
sudo apt install maven -y
```

Verification:
```
java --version
mvn --version
```


3. Building the Java Web Application
The source code was navigated to, and Maven was used to compile and package the application into a WAR file.

```
mvn package
```

The build was successful, and the JavaWebApp.war file was generated in the target/ directory.





Phase 2: Web Server Setup

1. Installing and Configuring GlassFish

GlassFish was downloaded and extracted to the /opt directory. Environment variables were set to make GlassFish commands accessible system-wide.

```
cd /opt/
wget -O glassfish-5.1.0.zip https://download.eclipse.org/glassfish/glassfish-5.1.0.zip
```

```
sudo tar -xzf glassfish-5.1.0.zip
echo "export GLASSFISH_HOME=/opt/glassfish5" | sudo tee -a /etc/environment
echo "export PATH=\$PATH:\$GLASSFISH_HOME/bin" | sudo tee -a /etc/environment
source /etc/environment
```



2. Setting Execution Permissions

The necessary execution permissions were granted to the GlassFish administration script.

```
sudo chmod +x /opt/glassfish5/bin/asadmin
```


3. Starting the GlassFish Domain

The GlassFish application server was started using the asadmin tool.
```
./asadmin start-domain
```



Phase 3: Artifact Transfer and Deployment

1. Transferring the WAR File
The built JavaWebApp.war artifact was securely copied from the build server to the home directory of the web server using scp with a PEM key for authentication.

```
scp -i maven.pem DevSecOps-tasks/Build-tools/javawebapp/target/JavaWebApp.war ubuntu@3.92.21.29:/home/ubuntu
```

2. Deploying the Application on GlassFish
From the web server, the asadmin tool was used to deploy the transferred WAR file to the running GlassFish instance.
```
./asadmin deploy /home/ubuntu/JavaWebApp.war
```

3. Verifying the Deployment
The deployment was confirmed by listing all applications on the GlassFish server. The JavaWebApp was shown as successfully deployed.

```
ubuntu@glassfish:/opt/glassfish5/bin$ ./asadmin list-applications
JavaWebApp <web>
Command list-applications executed successfully.
```


Phase 4: Access the application
Go to the web browser and give your ip-adress with portnumber and application name.
```
<ip-adress>:8080/JavaWebApp

Now you will be able to acess the application

