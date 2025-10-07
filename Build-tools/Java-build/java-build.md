Building and Deploying a Java Web Application using Maven and Apache Tomcat
1. Synopsis

This project demonstrates how to build a Java web application using Apache Maven on a build server, generate a .war (Web Application Archive) file, and then deploy the artifact to Apache Tomcat running on a web server.


## 2. Requirements

| Component     | Version      | Purpose                                     |
| ------------- | ------------ | ------------------------------------------- |
| Java (JDK)    | 17           | Required for Maven and Tomcat               |
| Apache Maven  | 3.6+         | Build automation tool                       |
| Apache Tomcat | 9            | Web server to deploy WAR file               |
| Git           | Latest       | Version control system                      |
| SCP / SSH     | -            | To transfer build artifacts between servers |
| Ports         | 22, 8080     | To access SSH and tomcat.                   |


Server Setup
| Server       | Role                        | Example Hostname / IP     |
| ------------ | --------------------------- | ------------------------- |
| Build Server | Runs Maven to build the app | `maven@<build-server-ip>` |
| Web Server   | Runs Tomcat to host the app | `ubuntu@<web-server-ip>`  |



## Directory Structure

JavaWebCalculator/
├── src/
│   └── main/
│       ├── java/
│       │   └── com/example/Calculator.java
│       └── webapp/
│           ├── index.jsp
│           └── WEB-INF/web.xml
├── pom.xml
└── README.md



4. Steps to Build and Deploy

Step 1: Install Dependencies

On both servers (Build and Web):

Build server:
```
sudo apt update
sudo apt install openjdk-17-jdk -y
sudo apt install maven -y
```

Web server:
```
sudo apt install openjdk-17-jdk -y
sudo apt install tomcat9 -y
```



Verify installations:

```
java -version
mvn -version
sudo systemctl status tomcat9
```


Step 2: Clone the Repository on Build Server
```
git clone https://github.com/<your-username>/<repo-name>.git
cd JavaWebCalculator
```


Step 3: Build the Project using Maven
--> check your pom.xml file if the versions are compatable. If not edit the file with compatable versions.

pom.xml:
```
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/maven-v4_0_0.xsd">

  <modelVersion>4.0.0</modelVersion>

  <groupId>com.web.cal</groupId>
  <artifactId>webapp</artifactId>
  <version>0.2</version>
  <packaging>war</packaging>
  <name>WebAppCal Maven Webapp</name>
  <url>http://maven.apache.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.8.2</version>
      <scope>test</scope>
    </dependency>

    <!-- Servlet API -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
      <version>2.5</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <!-- Modern WAR Plugin -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-war-plugin</artifactId>
        <version>3.3.2</version>
      </plugin>

      <!-- Compiler Plugin -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.11.0</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
        </configuration>
      </plugin>
    </plugins>
  </build>

</project>
```

Build the code:
```
mvn package
```

You will see the below output:


The successful build will create a target folder in which .war file is stored. check for it.

step 4: copy your .pem file to your build server.

```
cd /.downloads/
scp -i <key.pem> <key.pem> ubuntu@<ip-address>:~
```

Step 5: Transfer the WAR File to Web Server

But first, change your pemfile permissions.
```
sudo chmod 400 key.pem
```

Use SCP (Secure Copy Protocol) to move the artifact to Tomcat’s webapps directory on the web server:
```
scp -i <key.pem> target/webapp-0.1.war ubuntu@<web-server-ip>:/home/ubuntu/apache-tomcat-9.0.109/webapps/
```

Step 5: start the Apache tomcat server 
```
cd bin/
```
Add executable permissions to the file.

```
chmod +x bin/*.sh
```

Start the server
```
./startup.sh
```



Step 6: check the web server.
Go to web browser and give 
```
<ip-address:8080>
```
It should show you the tomcat configuration page.

Step 7: Complete the configurations given in tomcat page.

In web server, edit context.xml file for both managerand host-manager
```
sudo vi /home/ubuntu/apache-tomcat-9.0.109/webapps/manager/META-INF/context.xml
```
Comment the below shown line and save it.

Do the same for another file.
```
sudo vi /home/ubuntu/apache-tomcat-9.0.109/webapps/host-manager/META-INF/context.xml
```

Add User Roles in Tomcat Users File. 
Add this inside <tomcat-users>:
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<user username="admin" password="admin123" roles="manager-gui,manager-script"/>

You can change admin and admin123 to your preferred username/password.




Step 8: Restart Tomcat

After making changes:

```
cd /home/ubuntu/apache-tomcat-9.0.109/bin
./shutdown.sh
./startup.sh
```



Step 9: Verify Access

Open your browser and visit:

<your-ip-address:8080>

It will ask you for signin details. Give your signin details and you will be able to see tomcat page with the .war file name webapp-0.2 in this case.

Click on webapp-0.2 and you will access the application.

Step 10: Use the application

