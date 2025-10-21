Java Project Code Quality Testing with SonarQube and Maven on Ubuntu

This guide explains how to set up SonarQube on an Ubuntu server and analyze a Java project using Maven. The example project is JavaWebCalculator.

Prerequisites:
    Ubuntu server (18.04, 20.04, or newer)
    sudo privileges
    Internet access to download SonarQube and Java



Clone Java Project
Clone your project from GitHub:
```
git clone https://github.com/akracad/JavaWebCalculator.git
cd JavaWebCalculator/
```

Verify the source and test files exist:
```
ls src/main/java/mypackage/Calculator.java
ls src/test/java/mypackage/CalculatorTest.java
```

Install Java and Maven
For SonarQube 6 and modern Maven plugins, use Java 8:
```
sudo apt update
sudo apt install openjdk-8-jdk maven -y
```
Verify installations:
```
java -version
mvn --version
```


Install and Configure SonarQube
1. Download SonarQube
```
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-6.7.7.zip
sudo apt install unzip -y
unzip sonarqube-6.7.7.zip
```

2. Configure SonarQube

Edit the configuration file:
```
cd ~/sonarqube/conf/
vi sonar.properties
```
Set the web host and port:
```
# Listen on all interfaces
sonar.web.host=0.0.0.0
sonar.web.port=9000
```


3. Start SonarQube
```
cd ~/sonarqube/bin/linux-x86-64/
./sonar.sh start
./sonar.sh status
```

    Access SonarQube in a browser: http://<server-ip>:9000
    Default credentials: admin/admin.


Run Maven Build and SonarQube Analysis

1. Build the project:
```
mvn package
```

2. Run SonarQube analysis:
```
mvn sonar:sonar \
  -Dsonar.host.url=http://54.183.136.50:9000 \
  -Dsonar.login=e129e2e9a0aa3be7504c091c53379c7f241346ea \
  -Dsonar.projectKey=WebApp \
  -Dsonar.projectName="Web Application"
```

Access SonarQube Dashboard
```
http://54.183.136.50:9000/dashboard?id=WebApp
```

    Review bugs, code smells, vulnerabilities, and test coverage.
    Use your SonarQube token for authentication if required.


