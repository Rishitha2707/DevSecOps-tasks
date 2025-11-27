

# Jenkins CI/CD Automation Project
 
 
## Project Overview

This project demonstrates a fully automated **Jenkins CI/CD pipeline** for a Java web application.  

It integrates the following components:
 
- **Jenkins Master & Worker Nodes** for pipeline execution  

- **SonarQube** for code quality checks  

- **Nexus Repository Manager** for artifact storage  

- **Tomcat Server** for application deployment  

- **GitHub** repository as the source code  
 
The pipeline automatically performs **build, test, code quality scan, artifact upload, and deployment**, eliminating manual steps entirely.
 
---
 
## Architecture
 
**Master-Worker Jenkins Architecture**
 
```

                      +----------------------+

                   |    Jenkins Master    |

                   +----------+-----------+

                              |

        -------------------------------------------------

        |                       |                       |

+---------------+      +---------------+      +---------------+

|  SonarQube    |      |    Nexus      |      |    Tomcat     |

|    Worker     |      |    Worker     |      |    Worker     |

+---------------+      +---------------+      +---------------+
 
```
 
- Jenkins Master orchestrates jobs.  

- Worker nodes execute specific tasks like SonarQube scans, Nexus uploads, and Tomcat deployments.  
 
---
 
## Servers Setup
``` 
| Server       | Type        | Ports       | Purpose         |

|------------- |------------ |------------ |----------------|

| Jenkins      | t2.micro    | 22, 8080   | CI/CD Master    |

| SonarQube    | t2.large    | 22, 9000   | Code Quality    |

| Nexus        | t2.medium   | 22, 8081   | Artifact Repo   |

| Tomcat       | t2.micro    | 22, 8080   | Application Deploy |
 ```

## Create jenkins-server
 
<img width="602" height="273" alt="image" src="https://github.com/user-attachments/assets/75600db3-3656-4130-841c-1d96afc5d740" />
 
<img width="602" height="273" alt="image" src="https://github.com/user-attachments/assets/56f5fb9b-6240-4d66-8308-2cea7a91764a" />
 
<img width="602" height="248" alt="image" src="https://github.com/user-attachments/assets/49efb2db-0240-467e-bb9e-24b2f7747421" />
 
<img width="602" height="255" alt="image" src="https://github.com/user-attachments/assets/22468009-5c8d-4db7-bad9-3a612608c5aa" />
 
## Create sonar-server
 
<img width="602" height="282" alt="image" src="https://github.com/user-attachments/assets/dba2c3b7-561a-4e6e-a754-96a75bc8997e" />
 
<img width="602" height="282" alt="image" src="https://github.com/user-attachments/assets/a57dd2be-9e7e-4334-a283-9d8557e47fa6" />
 
<img width="602" height="240" alt="image" src="https://github.com/user-attachments/assets/60a728ee-75e6-407b-a569-40c1c2129e7f" />
 
<img width="602" height="264" alt="image" src="https://github.com/user-attachments/assets/925a71e8-3194-4640-bc69-cd8b950086a9" />
 
 
## Create nexus-server
 
<img width="602" height="252" alt="image" src="https://github.com/user-attachments/assets/8574287a-a3dc-45e4-8f27-66b1fbb8f5bc" />
 
<img width="602" height="90" alt="image" src="https://github.com/user-attachments/assets/2fa6e505-8480-4734-888b-9642e7b4db97" />
 
<img width="602" height="256" alt="image" src="https://github.com/user-attachments/assets/3faca7c2-e3ff-475d-a641-28d9da402129" />
 
<img width="602" height="203" alt="image" src="https://github.com/user-attachments/assets/14ce0ef6-982a-46ef-ad89-ceb30f342d18" />
 
 
## Create tomcat-server
 
<img width="602" height="251" alt="image" src="https://github.com/user-attachments/assets/ca8063e1-7bb4-4a44-b45c-f3e5dad77830" />
 
<img width="602" height="233" alt="image" src="https://github.com/user-attachments/assets/20f8d43d-de82-4c51-b719-80273f0b7174" />
 
<img width="602" height="225" alt="image" src="https://github.com/user-attachments/assets/dbe8cca7-c51d-4a3a-9486-924bade52cd4" />
 
- **Hostname Update** (for all servers):

```bash

sudo hostnamectl set-hostname <server-name>

sudo init 6

````
<img width="602" height="67" alt="image" src="https://github.com/user-attachments/assets/055cbc88-a56e-4032-8270-891d16806f45" />
 
<img width="602" height="92" alt="image" src="https://github.com/user-attachments/assets/4615f64d-457e-4fa6-aa8e-d15848e11416" />
 
<img width="602" height="91" alt="image" src="https://github.com/user-attachments/assets/41266629-4d78-4ac6-9647-1617ed0ee362" />

 ## Update all the serveres and install java (install maven also on build server)
 
```

sudo apt-get update -y

sudo apt-get install -y openjdk-17-jdk

java –version 

```

* **Java Installation**
 
  * Jenkins/Tomcat: OpenJDK 17 or 21 depending on server

  * SonarQube: OpenJDK 17 + Maven

  * Nexus: OpenJDK 17
 
---
<img width="602" height="122" alt="image" src="https://github.com/user-attachments/assets/a62bc409-f9cd-4324-acd6-fbfdc0fa5232" />
 
<img width="602" height="74" alt="image" src="https://github.com/user-attachments/assets/d5670f90-0f6b-42b2-90bb-444fea97499f" />
 
<img width="602" height="64" alt="image" src="https://github.com/user-attachments/assets/df1891dd-d6f1-48d6-a2b6-5e7a3657d0a3" />
 
## Software Installation & Configuration
 
### Jenkins Server
 
```bash

sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list

sudo apt update

sudo apt install jenkins -y

sudo systemctl enable jenkins

sudo systemctl start jenkins

```
 
<img width="602" height="276" alt="image" src="https://github.com/user-attachments/assets/0959b5ba-8974-4572-987b-c43da9576324" />
 
 
* Access: `http://<JENKINS_PUBLIC_IP>:8080`
 
<img width="602" height="291" alt="image" src="https://github.com/user-attachments/assets/de3cdf56-7ca2-47d9-a3a2-9b4e8160b291" />
 
* Unlock Jenkins: `/var/lib/jenkins/secrets/initialAdminPassword`
 
<img width="602" height="38" alt="image" src="https://github.com/user-attachments/assets/00990b7a-0419-4391-b9ee-db701832cf85" />
 
* Install suggested plugins and create the first admin user
 
<img width="602" height="315" alt="image" src="https://github.com/user-attachments/assets/a7b78344-0eb3-49f3-b898-9491c62d7ebd" />
 
<img width="602" height="277" alt="image" src="https://github.com/user-attachments/assets/c3c58e70-2d90-47ce-a65b-55c73515d215" />
 
* Here you have to create the first admin user with username and password
 
<img width="602" height="355" alt="image" src="https://github.com/user-attachments/assets/84f00406-aac3-4385-87e6-45f6e7b29c69" />
 
Save and finish it.
 
<img width="602" height="355" alt="image" src="https://github.com/user-attachments/assets/03b38461-4840-4f78-9ea8-a6c09444525b" />
 
 
* Jenkins is ready, start using it:
 
<img width="602" height="168" alt="image" src="https://github.com/user-attachments/assets/843ba7f0-a238-4923-9d3c-43b045f2372d" />
 
* 

You will see the Jenkins dashboard
 
<img width="602" height="290" alt="image" src="https://github.com/user-attachments/assets/591b74f0-eac7-4414-bd19-24cdf7519991" />
 
<img width="602" height="284" alt="image" src="https://github.com/user-attachments/assets/2f36d0bc-b5c1-4608-94fb-1be75d2bce58" />
 
### SonarQube Server
 
```bash

cd /opt

sudo useradd -m -d /opt/sonarqube -r -s /bin/bash sonar

sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.6.0.92116.zip

sudo unzip sonarqube-10.6.0.92116.zip

sudo mv sonarqube-10.6.0.92116 sonarqube

sudo chown -R sonar:sonar /opt/sonarqube

```
<img width="602" height="213" alt="image" src="https://github.com/user-attachments/assets/c6effaae-c7da-4a65-b1cc-e7a3efb1fa12" />
 
<img width="602" height="48" alt="image" src="https://github.com/user-attachments/assets/5ea6e65d-1476-42c1-9031-5a1b94896a8f" />
 
 
* Configure `sonar.properties` in `/opt/sonarqube/conf/`
 
<img width="602" height="138" alt="image" src="https://github.com/user-attachments/assets/097f5382-8ecf-437f-a01a-36eb6b930779" />
 
<img width="602" height="289" alt="image" src="https://github.com/user-attachments/assets/20c01e48-80aa-44d4-b30e-3cb536ed6d5e" />
 
<img width="602" height="285" alt="image" src="https://github.com/user-attachments/assets/cf134a54-c5b5-4bde-b6be-a4bfae8d44d2" />
 
<img width="602" height="285" alt="image" src="https://github.com/user-attachments/assets/c1cea6f5-7ab0-4af7-b90f-4c233c86c948" />
 
 


### Nexus Server
 
```bash

cd /home/ubuntu

wget https://download.sonatype.com/nexus/3/nexus-3.85.0-03-linux-x86_64.tar.gz

tar -xvzf nexus-3.85.0-03-linux-x86_64.tar.gz

cd nexus-3.85.0-03

bin/nexus start

bin/nexus status

```
 
<img width="602" height="265" alt="image" src="https://github.com/user-attachments/assets/2f35ad98-666b-4246-8a3e-49d313ac0a0c" />
 
<img width="602" height="266" alt="image" src="https://github.com/user-attachments/assets/5047c969-f773-45d6-869b-5348d9db1cc8" />
 
<img width="602" height="88" alt="image" src="https://github.com/user-attachments/assets/d8b31f3b-ccef-4c16-9ec2-3abda8fc28af" />
 
 
* Access: `http://<NEXUS_PUBLIC_IP>:8081`
 
<img width="602" height="289" alt="image" src="https://github.com/user-attachments/assets/156e1086-e380-4620-b1e8-d27242646067" />
 
* Default login: `admin`

* Password: `cat /home/ubuntu/sonatype-work/nexus3/admin.password`
 
<img width="602" height="300" alt="image" src="https://github.com/user-attachments/assets/87693e12-9a1c-43d4-be71-04b27cfff5a9" />
 
 
### Tomcat Server
 
```bash

cd /home/ubuntu

wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.109/bin/apache-tomcat-9.0.109.tar.gz

tar -xvzf apache-tomcat-9.0.109.tar.gz

cd apache-tomcat-9.0.109

./bin/startup.sh

```
 
<img width="602" height="183" alt="image" src="https://github.com/user-attachments/assets/63ea7719-f50e-4a06-ba5f-13f293a26571" />
 
<img width="602" height="166" alt="image" src="https://github.com/user-attachments/assets/5cb8be5f-b177-48b1-858d-8e5b2e699cbc" />
 
* Deploy WAR:
 
```bash

sudo cp webapp-add-0.0.1.war /home/ubuntu/apache-tomcat-9.0.109/webapps/

tail -f logs/catalina.out

```
 
* Configure Tomcat Manager (optional):
 
* Add admin user in `tomcat-users.xml`
<img width="602" height="88" alt="image" src="https://github.com/user-attachments/assets/7c2e8fb5-4ce6-45cf-88f6-fd98254cfd71" />
 
* Update `context.xml` to allow trusted IP access
 
Edit manager context.xml files if required:

```

sudo vi /home/ubuntu/apache-tomcat-9.0.109/webapps/manager/META-INF/context.xml

sudo vi /home/ubuntu/apache-tomcat-9.0.109/webapps/host-manager/META-INF/context.xml

```
 
Remove any 127.0.0.0 restriction or any IP-based <Valve> that prevents access from your IP (

```

/home/ubuntu/apache-tomcat-9.0.109/bin/shutdown.sh

/home/ubuntu/apache-tomcat-9.0.109/bin/startup.sh

```

be careful — only allow trusted IPs in production).

Restart Tomcat:
 
<img width="602" height="161" alt="image" src="https://github.com/user-attachments/assets/ee2e143d-125e-4d79-abe6-2460cb14c024" />
 
<img width="602" height="320" alt="image" src="https://github.com/user-attachments/assets/93c96c69-9c49-4278-a9eb-4dc2f162a2cf" />
 
---
 
## Jenkins Configuration
 
<img width="602" height="271" alt="image" src="https://github.com/user-attachments/assets/b691ff0d-84a0-420d-91b9-518a1ecb3620" />
 
 
### Plugins Installed
 
* GitHub Integration & Authentication

* Maven Integration

* SonarQube Scanner & Quality Gates

* Artifact Deployer & Nexus Artifact Uploader

* SSH, SSH Agent, SSH Pipeline Steps

* Publish over SSH
 
<img width="602" height="300" alt="image" src="https://github.com/user-attachments/assets/ad4d6038-9bd9-45fb-8c89-7fb68b64d3b2" />
 
<img width="602" height="298" alt="image" src="https://github.com/user-attachments/assets/2410ac51-2f6d-4bee-ae3b-4aeba731821c" />
 
<img width="602" height="300" alt="image" src="https://github.com/user-attachments/assets/5d719d7d-2416-42c3-8b68-44b58120621a" />
 
 
### Tools Configuration
 
* Java & Maven paths configured

* Worker nodes setup with SSH keys for Jenkins master
 
<img width="602" height="44" alt="image" src="https://github.com/user-attachments/assets/f246cd8e-7a34-4cb7-a063-d4d209ab54e2" />
 
<img width="602" height="50" alt="image" src="https://github.com/user-attachments/assets/04acb075-b5e8-477f-ba50-5174491cc2e9" />
 
<img width="602" height="286" alt="image" src="https://github.com/user-attachments/assets/23c6a1e9-f371-4599-aa73-e036a0154cc4" />
 
<img width="602" height="35" alt="image" src="https://github.com/user-attachments/assets/1151d99b-6d24-4db8-9cbd-b98e4e6ebd07" />
 
<img width="602" height="192" alt="image" src="https://github.com/user-attachments/assets/d6167f0c-f98c-4a3f-ada7-057c301c7580" />
 
 ### Worker Nodes
 
* Created `jenkins` folder in all worker servers

```

mkdir jenkins

```
<img width="602" height="112" alt="image" src="https://github.com/user-attachments/assets/82cf8246-9e06-4440-b97f-837e61a24277" />
 
<img width="602" height="89" alt="image" src="https://github.com/user-attachments/assets/79c2cced-0ad4-4867-a9d1-7d7f2cd864e6" />
 
<img width="602" height="117" alt="image" src="https://github.com/user-attachments/assets/810aadfa-dc54-477c-93e9-a3b2092a6dbc" />
 
* Then go to jenkin-server and switch user as Jenkins user and generate the ssh keys as shown below.
 
```

sudo su Jenkins

```
<img width="602" height="318" alt="image" src="https://github.com/user-attachments/assets/c522f79f-77c8-48be-918c-6c22a9e178ef" />
 
* Copied Jenkins master public key to worker `.ssh/authorized_keys` on all the sonar, nexus and tomcat servers
 
<img width="602" height="152" alt="image" src="https://github.com/user-attachments/assets/fa62a28f-0daf-4009-9747-8006bc9c40c7" />
 
<img width="602" height="119" alt="image" src="https://github.com/user-attachments/assets/8526ca66-ac9e-4852-b071-d6ff72b78cf3" />
 
* Verified connectivity from jenkins server : `ssh ubuntu@<worker-public-ip>`
 
```

ssh ubuntu@<public ip of worker server>

```
<img width="602" height="340" alt="image" src="https://github.com/user-attachments/assets/e6fd11eb-6c5a-4a2f-aa92-fc27ca2a95c2" />
 
<img width="602" height="276" alt="image" src="https://github.com/user-attachments/assets/69f01d87-7af1-4dc0-8043-89e5ef35cf68" />
 
<img width="602" height="287" alt="image" src="https://github.com/user-attachments/assets/0afc5713-c2d5-41f2-8967-5bca9ae69174" />
 
 
### Credentials Setup
 
<img width="602" height="182" alt="image" src="https://github.com/user-attachments/assets/73241e52-7455-40f9-82b3-ccd76409bf03" />
 
 
* SonarQube token
 
<img width="602" height="270" alt="image" src="https://github.com/user-attachments/assets/1f0fa10b-bdaf-4fa7-80a8-b307d723f394" />
 
<img width="602" height="115" alt="image" src="https://github.com/user-attachments/assets/d1c60290-c397-4936-ae85-febc56894453" />
 
* Nexus and tomcat username/password
 
<img width="602" height="283" alt="image" src="https://github.com/user-attachments/assets/0c76571c-b183-4356-a9a2-70cab2c3bf3d" />
 
<img width="602" height="280" alt="image" src="https://github.com/user-attachments/assets/42595df6-ab97-4e92-96b8-1d2be8188c9d" />
 
<img width="602" height="183" alt="image" src="https://github.com/user-attachments/assets/3b7dea6a-a61c-449a-a44d-96c77f7f533f" />
 
* Jenkins SSH private key
 
<img width="602" height="251" alt="image" src="https://github.com/user-attachments/assets/4162c950-80c5-4189-9310-2a2ece19b05d" />
 
<img width="602" height="246" alt="image" src="https://github.com/user-attachments/assets/df4b1c8a-c9ec-4417-a7b6-d6dcd8d43b42" />
 
<img width="602" height="200" alt="image" src="https://github.com/user-attachments/assets/e8e6af71-a9df-4c5d-bb90-1308c7a6faba" />
 
 
### Configure Jenkins Nodes (Agent Setup)
 
In this step, we configure worker nodes (agents) for Jenkins to delegate specific stages —

SonarQube scanning, artifact uploading to Nexus, and deployment on Tomcat.
 
1️⃣ Navigate to Jenkins Nodes Configuration
 
From the Jenkins dashboard, go to:

Manage Jenkins → Nodes and Clouds → New Node
 
<img width="602" height="138" alt="image" src="https://github.com/user-attachments/assets/1fe35b2f-e47a-4c51-a741-34a896657302" />
 
 
Click “New Node”, then provide the details as follows.
 
2️⃣ Create Sonar Node
 
Enter Node Name: sonar-node
 
Select Permanent Agent, then click OK
 
<img width="602" height="190" alt="image" src="https://github.com/user-attachments/assets/8c0adea2-6c4c-43d3-b961-9ba40f62e449" />
 
 
Fill in:
 
Remote root directory: /home/ubuntu/jenkins
 
<img width="602" height="258" alt="image" src="https://github.com/user-attachments/assets/22abb455-5302-42c5-b130-55fee5655c86" />
 
 
Labels: sonar
 
Launch method: “Launch agents via SSH”
 
Host: <SONARQUBE_PUBLIC_IP>
 
Credentials: Select the SSH key added earlier for Jenkins
 
<img width="602" height="202" alt="image" src="https://github.com/user-attachments/assets/46467328-6b67-4178-977d-15f5a381ca4f" />
 
 
Click Save and Apply
 
Jenkins will automatically connect to the SonarQube server.
 
<img width="602" height="167" alt="image" src="https://github.com/user-attachments/assets/303def24-1a77-47ad-a568-f92fd27734a4" />
 
 
✅ Verification:
 
After saving, refresh the node list — you should see sonar-node showing “Connected”.
 
Click on the node → Log tab to confirm:
 
Agent successfully connected and online
 
<img width="602" height="244" alt="image" src="https://github.com/user-attachments/assets/53a470eb-0eb8-4b9e-bfba-fdd6225d1a5a" />
 
3️⃣ Repeat for Nexus Node
 
Follow the same steps for the Nexus server:
 
Node Name: nexus-node
 
Labels: nexus
 
Host: <NEXUS_PUBLIC_IP>
 
Credentials: Jenkins SSH private key
 
Save and confirm that the node is online.
 
<img width="602" height="198" alt="image" src="https://github.com/user-attachments/assets/232a447b-0fa4-4c37-af8b-e184895a8820" />
 
<img width="602" height="182" alt="image" src="https://github.com/user-attachments/assets/b6e146fc-3780-4dcb-9c88-3841fa07f5a6" />
 
 
✅ Check logs to verify:
 
Agent nexus-node connected successfully
 
<img width="602" height="260" alt="image" src="https://github.com/user-attachments/assets/91604182-d14b-4bad-a6a3-f27fc151a411" />
 
4️⃣ Repeat for Tomcat Node
 
Finally, create the deployment node:
 
Node Name: tomcat-node
 
Labels: tomcat
 
Host: <TOMCAT_PUBLIC_IP>
 
Credentials: Jenkins SSH private key
 
Save the configuration.
 
<img width="602" height="192" alt="image" src="https://github.com/user-attachments/assets/6faff499-46f6-4264-b4b6-36e6b717d208" />
 
<img width="602" height="222" alt="image" src="https://github.com/user-attachments/assets/17d2c110-d476-43c9-a885-1ea1090af040" />
 
 
✅ Confirm in logs:
 
Agent tomcat-node connected successfully
 
<img width="602" height="267" alt="image" src="https://github.com/user-attachments/assets/e3c7a734-4f04-4c59-953a-204edce1aac5" />
 
Now you can see all the nodes.
 
<img width="602" height="213" alt="image" src="https://github.com/user-attachments/assets/e793ce30-291c-4f8d-9c79-17f73a3ba337" />

 ## Pipeline Setup
 
<img width="602" height="277" alt="image" src="https://github.com/user-attachments/assets/baf0b881-d207-4f2a-bce7-41b1d6df5a7e" />
 
 
🧩 End-to-End Pipeline Flow in Real-Time
 
* Stages:
 
  1. **Build** - Maven build of Java project

  2. **SonarQube** - Quality scan & quality gate validation

  3. **Nexus** - Artifact upload

  4. **Tomcat** - WAR deployment
 
* Jobs will executed sequentially across nodes using labels
 
Step 1: Jenkins pulls the code from GitHub:

🔗 Java Web Calculator App Repo
 
Step 2: Runs the stages defined in Jenkinsfile:
 
Build → Sonar → Nexus → Tomcat
 
 
## GitHub Integration
 
* Repository: [Java Web Calculator App](https://github.com/SaiKumar7596/Java-Web-Calculator-App)
 
* This is the code repo where my project is coming from
 
```
https://github.com/SaiKumar7596/Java-Web-Calculator-App

```
 
 
---
<img width="602" height="297" alt="image" src="https://github.com/user-attachments/assets/b8b67f53-6ed9-486a-a408-93001943a0cb" />
 
<img width="602" height="112" alt="image" src="https://github.com/user-attachments/assets/64f2ca43-54e1-4912-a87b-0416e37d3a7b" />
 
 
Step 3: Edit settings.xml on Sonar server to match your Nexus credentials:

```
<server>
<id>nexus</id>
<username>admin</username>
<password>admin123</password>
</server>

```
<img width="602" height="85" alt="image" src="https://github.com/user-attachments/assets/736039a3-c485-49de-beb0-f404a255ee41" />
 
<img width="602" height="135" alt="image" src="https://github.com/user-attachments/assets/aa503105-b09a-43e6-bb8e-632abbf9322d" />
 
Ensure the <id> matches the one used in the Jenkins credentials for Nexus.

This ensures Maven can authenticate and push artifacts automatically.
 
Step 4: Assign correct labels in Jenkinsfile:
 
agent { label 'tomcat' }
 
<img width="602" height="113" alt="image" src="https://github.com/user-attachments/assets/72b7db21-7407-49e4-9de0-9539501dadd2" />
 
<img width="602" height="215" alt="image" src="https://github.com/user-attachments/assets/89d4518e-e6de-47de-b2de-62d8237597dc" />
 
<img width="602" height="185" alt="image" src="https://github.com/user-attachments/assets/02089089-6481-47d1-a2d6-0be6e6c32351" />
 
<img width="602" height="103" alt="image" src="https://github.com/user-attachments/assets/7cf44642-2484-4908-b03d-88665f53afda" />
 
<img width="602" height="68" alt="image" src="https://github.com/user-attachments/assets/dd6054da-fc3e-4408-9977-29b59853b7f1" />
 
<img width="602" height="128" alt="image" src="https://github.com/user-attachments/assets/71f9aba5-ce7b-4b76-ab07-81c1fff5b26a" />
 
<img width="602" height="110" alt="image" src="https://github.com/user-attachments/assets/cd804073-a9cd-4b02-a2a5-b6d71502d8d9" />
 
 
This ensures the deployment stage runs on the Tomcat node.
 
🧠 Example Code Change & Version Deployment
 
Let’s simulate a code change for version updates:
 
Open the file

src/main/webapp/index.jsp
 
Remove subtraction and multiplication operations (for version 1)
 
Commit and push changes to GitHub
 
Go back to Jenkins and click Build Now
 
✅ Version 1 of your application is automatically built, analyzed, and deployed to Tomcat.
 
<img width="602" height="213" alt="image" src="https://github.com/user-attachments/assets/6c80626e-8410-44d0-aaeb-87746e7243cf" />
 
<img width="602" height="263" alt="image" src="https://github.com/user-attachments/assets/da1974fc-5fe0-4f8f-ab9d-082a21cb03b9" />
 
<img width="602" height="227" alt="image" src="https://github.com/user-attachments/assets/44f233c9-7b90-4544-9c1a-183a3b6c1de1" />
 
<img width="602" height="291" alt="image" src="https://github.com/user-attachments/assets/0efb1997-b996-4449-9e68-90fbbba47324" />
 
 
Visit:
 
http://<TOMCAT_PUBLIC_IP>:8080/Java-Web-Calculator-App
 
 
You’ll see the updated version of the app.
 
<img width="602" height="42" alt="image" src="https://github.com/user-attachments/assets/95d87417-49f1-4f3b-b1d3-fed4a4381bf8" />
 
<img width="602" height="81" alt="image" src="https://github.com/user-attachments/assets/5e3aff28-ffaf-4e61-8575-226dba5c8bef" />
 
 
Repeat similar steps for Version 2 (adding subtraction) and Version 3 (adding multiplication).
 
Version @2 of Substraction
 
<img width="602" height="216" alt="image" src="https://github.com/user-attachments/assets/0403ab2d-72f2-4890-ae51-28d448274984" />
 
<img width="602" height="111" alt="image" src="https://github.com/user-attachments/assets/7210863a-7200-488c-953b-c365f08fcf7a" />
 
 
⚡ Automating with GitHub Webhook
 
For the third version, make the process fully automated by setting up a GitHub webhook.
 
Steps:

```

Go to your GitHub repository → Settings → Webhooks
 
Click Add Webhook
 
Payload URL:
 
http://<JENKINS_PUBLIC_IP>:8080/github-webhook/
 
Content type: application/json
 
Select: “Just the push event”
 
Click Add Webhook

```
<img width="602" height="266" alt="image" src="https://github.com/user-attachments/assets/ec1f2f6e-2e73-4437-b177-fa8e40a427fe" />
 
Now add 'GitHub hook trigger for GITSam poling' on pipeline
 
 
Now, every time you push code changes, Jenkins automatically triggers the pipeline.
 
✅ Build → Analyze → Upload → Deploy — all without manual intervention!

* Pipeline as Code (Jenkinsfile) implemented
 
 
* Refresh the Jenkins server:
 
<img width="602" height="260" alt="image" src="https://github.com/user-attachments/assets/c39e890e-d687-4876-9f33-90a833377ca0" />
 
 
<img width="602" height="122" alt="image" src="https://github.com/user-attachments/assets/1f158ea9-fead-4d47-99e9-94e9341526b2" />
 
 
## Testing & Deployment
 
Version 1: Manual build & deploy
 
Version 2: Manual build after code changes
 
Version 3: Automated build via GitHub webhook
 
Verified deployment via Tomcat public IP:
 
http://<TOMCAT_PUBLIC_IP>:8080
 
 
## Best Practices & Notes
 
* Ensure correct AWS region for all EC2 instances

* Configure security groups properly for ports 22, 8080, 8081, 9000

* Use separate users for running SonarQube and Nexus in production

* Verify all services before running pipeline

* Use Jenkins credentials securely for tokens & SSH keys
 
✅ Final Outcome: End-to-End Jenkins CI/CD Automation (Pipeline as Code)
 
This project demonstrates a fully automated, end-to-end CI/CD pipeline using Jenkins Pipeline as Code.

From code commit to deployment, every stage — build, code quality analysis (SonarQube), artifact management (Nexus), and deployment (Tomcat) — is completely automated without any manual intervention.
 
This implementation reflects a real-world enterprise CI/CD workflow, ensuring:
 
Continuous Integration through Git-based Jenkins pipelines
 
Continuous Delivery via Nexus and Tomcat nodes
 
Full automation and scalability using distributed Jenkins agents

 
