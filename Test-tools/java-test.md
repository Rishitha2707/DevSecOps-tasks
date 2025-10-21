**Java Project Code Quality Testing with SonarQube and Maven on Ubuntu**

This guide explains how to set up SonarQube on an Ubuntu server and analyze a Java project using Maven. The example project is JavaWebCalculator.

**Prerequisites:**
    Ubuntu server (18.04, 20.04, or newer)
    sudo privileges
    Internet access to download SonarQube and Java



**Clone Java Project**
Clone your project from GitHub:
```
git clone https://github.com/akracad/JavaWebCalculator.git
cd JavaWebCalculator/
```
<img width="1042" height="220" alt="Image" src="https://github.com/user-attachments/assets/0b937da8-2b89-4434-a1ec-793fcd46ff1e" />


Verify the source and test files exist:
```
ls src/main/java/mypackage/Calculator.java
ls src/test/java/mypackage/CalculatorTest.java
```

**Install Java and Maven**
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
<img width="981" height="117" alt="Image" src="https://github.com/user-attachments/assets/38cfc7ee-b04c-40b5-8fce-5a7a8e25a066" />
<img width="1160" height="173" alt="Image" src="https://github.com/user-attachments/assets/343a5efc-5417-41ef-9b1d-5269fb259cb7" />


**Install and Configure SonarQube**
1. Download SonarQube
```
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-6.7.7.zip
sudo apt install unzip -y
unzip sonarqube-6.7.7.zip
```
<img width="1849" height="267" alt="Image" src="https://github.com/user-attachments/assets/7b37c9cb-f651-44be-b209-1a5eff369437" />


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
<img width="909" height="221" alt="Image" src="https://github.com/user-attachments/assets/823dee50-d255-493d-bbf1-7baf4236ed89" />

    Access SonarQube in a browser: http://<server-ip>:9000
    Default credentials: admin/admin.

<img width="1154" height="262" alt="Image" src="https://github.com/user-attachments/assets/5ea1dea4-3620-4ba3-989f-ad34059d93e9" />

<img width="1780" height="665" alt="Image" src="https://github.com/user-attachments/assets/373f7cbc-c8fe-487b-8549-f82cfe72723d" />



**Run Maven Build and SonarQube Analysis**

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

<img width="1076" height="281" alt="Image" src="https://github.com/user-attachments/assets/a6a04d60-20ed-4ede-a3b5-0585a2767ddb" />

<img width="1010" height="193" alt="Image" src="https://github.com/user-attachments/assets/06c57c27-8c3e-4087-a935-0d35d4807b94" />


**Access SonarQube Dashboard**
```
http://54.183.136.50:9000/dashboard?id=WebApp
```

    Review bugs, code smells, vulnerabilities, and test coverage.
    Use your SonarQube token for authentication if required.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/af43b344-8cb2-47a8-adc1-10587e4e0a46" />

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/f7d47d88-223f-4300-98e7-87be39ab10c1" />

