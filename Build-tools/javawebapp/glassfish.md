**Project: Java Web Application with Maven and GlassFish**

**Synopsis**
This project demonstrates a Build Server compiles and packages a Java web application into a WAR file using Apache Maven (with Java 8). The resulting artifact is then securely transferred to a Web Server, where it is deployed and hosted on the GlassFish 5 application server. This process automates the build and deployment stages, showcasing a core DevOps workflow.

**Requirements**
------------------

**Build Server:**

    Ubuntu 24.04 LTS

    Java 8 (OpenJDK 1.8.0)

    Apache Maven 3.8.7

    Git

<img width="1188" height="568" alt="Image" src="https://github.com/user-attachments/assets/f7749017-4aa9-47ba-8f3e-914aa6751cb5" />

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/d298ef53-e97c-4f01-aa7b-747a9e734769" />

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/3503ec30-fe23-4153-b1a5-d5c8d8eb5431" />

**Web Server:**

    Ubuntu 24.04 LTS

    GlassFish Server 5.1.0

    Java (to run GlassFish)

**Network:**

    SSH access between servers.

    A PEM key file for secure SCP file transfer.



**Implementation Steps**
-----------------------------


**Phase 1: Build Server Setup**

1. Installing Java 8
The build server required a specific version of Java. Java 8 was installed using apt.
```
sudo apt update -y
sudo apt install openjdk-8-jre-headless -y
```

<img width="1047" height="352" alt="Image" src="https://github.com/user-attachments/assets/ea98aa33-606a-4d07-8415-7e3cec136095" />

2. Installing Maven
```
sudo apt install maven -y
```

Verification:
```
java --version
mvn --version
```
<img width="1164" height="276" alt="Image" src="https://github.com/user-attachments/assets/0f50e4c7-859d-4b84-8944-7d8bcdd029a8" />

3. Building the Java Web Application
The source code was navigated to, and Maven was used to compile and package the application into a WAR file.

```
mvn package
```

The build was successful, and the JavaWebApp.war file was generated in the target/ directory.

<img width="937" height="377" alt="Image" src="https://github.com/user-attachments/assets/84599c20-9817-4a42-83cd-8b2a8f4cbb76" />

<img width="735" height="65" alt="Image" src="https://github.com/user-attachments/assets/95b613f4-5a8f-419f-b8eb-fb4af8f832e7" />

**Phase 2: Web Server Setup**

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

<img width="933" height="285" alt="Image" src="https://github.com/user-attachments/assets/05de00c7-f7b1-4850-8eef-862af378bd6a" />

2. Setting Execution Permissions

The necessary execution permissions were granted to the GlassFish administration script.

```
sudo chmod +x /opt/glassfish5/bin/asadmin
```
<img width="945" height="170" alt="Image" src="https://github.com/user-attachments/assets/5fe998ed-bec7-4c32-8885-ecdcc6be64c7" />

3. Starting the GlassFish Domain

The GlassFish application server was started using the asadmin tool.
```
./asadmin start-domain
```
<img width="904" height="180" alt="Image" src="https://github.com/user-attachments/assets/5d380ed4-9440-4246-873d-4807a6e12026" />

output should be:
<img width="1919" height="617" alt="Image" src="https://github.com/user-attachments/assets/e522ecda-1977-4d66-8328-adc04e6ecf41" />

**Phase 3: Artifact Transfer and Deployment**

1. Transferring the WAR File
The built JavaWebApp.war artifact was securely copied from the build server to the home directory of the web server using scp with a PEM key for authentication.

```
scp -i maven.pem DevSecOps-tasks/Build-tools/javawebapp/target/JavaWebApp.war ubuntu@3.92.21.29:/home/ubuntu
```
<img width="923" height="73" alt="Image" src="https://github.com/user-attachments/assets/60fd64c5-3532-4959-a9e9-d8e8c203b413" />

2. Deploying the Application on GlassFish
From the web server, the asadmin tool was used to deploy the transferred WAR file to the running GlassFish instance.
```
./asadmin deploy /home/ubuntu/JavaWebApp.war
```
<img width="798" height="94" alt="Image" src="https://github.com/user-attachments/assets/7751a256-feaa-4ebc-ab55-d1b07af624a6" />

3. Verifying the Deployment
The deployment was confirmed by listing all applications on the GlassFish server. The JavaWebApp was shown as successfully deployed.

```
ubuntu@glassfish:/opt/glassfish5/bin$ ./asadmin list-applications
JavaWebApp <web>
Command list-applications executed successfully.
```


**Phase 4: Access the application**
Go to the web browser and give your ip-adress with portnumber and application name.
```
<ip-adress>:8080/JavaWebApp
```

Now you will be able to acess the application

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/10588961-b338-48f6-90ba-ca518508944f" />

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/39eeb8f2-5776-4bb8-ba31-066e36a15ee3" />


