📘 Jenkins Installation and Configuration on Ubuntu


## 🧩 Introduction

[Jenkins](https://www.jenkins.io/) is an open-source automation server used to automate building, testing, and deploying software.  
It helps implement **Continuous Integration (CI)** and **Continuous Deployment (CD)** pipelines.

---

## ⚙️ System Requirements

Before installation, ensure your system meets these requirements:

| Component | Requirement                     |
| ---------- | ------------------------------- |
| OS         | Ubuntu 20.04 / 22.04 LTS        |
| RAM        | Minimum 2 GB (4 GB recommended) |
| CPU        | 2 cores minimum                 |
| Disk       | 10 GB free space                |
| Java       | OpenJDK 21                      |
| Port       | 8080                            |

---

<img width="1919" height="627" alt="Image" src="https://github.com/user-attachments/assets/dd7b9b86-ac9d-438b-a6ec-924e490177e7" />


## 🚀 Step 1: Update System Packages

Update and upgrade your system before installation:

```
sudo apt update -y
sudo apt upgrade -y
```
<img width="876" height="159" alt="Image" src="https://github.com/user-attachments/assets/860fea92-2c78-4c25-b93a-024a1a60e091" />


**Verify installation:**

```
java --version
```

**Expected output:**
```
openjdk 21.0.8 2025-07-15
OpenJDK Runtime Environment (build 21.0.8+9-Debian-1)
OpenJDK 64-Bit Server VM (build 21.0.8+9-Debian-1, mixed mode, sharing)
```
<img width="1070" height="127" alt="Image" src="https://github.com/user-attachments/assets/6b6929f1-f12e-4700-accc-89ebd949683e" />


**📦 Step 3: Add Jenkins Repository and Key**
```
sudo mkdir -p /etc/apt/keyrings
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/" | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install jenkins -y
```
<img width="1283" height="421" alt="Image" src="https://github.com/user-attachments/assets/1e69d370-d7e6-4c37-bba6-0adf6c064acf" />

<img width="465" height="50" alt="Image" src="https://github.com/user-attachments/assets/78abeb39-274e-45f8-bae5-a41d8010a9e3" />


**🔄 Step 4: Start and Enable Jenkins Service**

Start Jenkins:
```
sudo systemctl start jenkins
```

Enable Jenkins to start on boot:
```
sudo systemctl enable jenkins
```

Check Jenkins status:
```
sudo systemctl status jenkins
```
<img width="1907" height="611" alt="Image" src="https://github.com/user-attachments/assets/d1309c77-bddc-448c-be71-30744a86b137" />


**🌐 Step 5: Check if Jenkins is Running on Port 8080**

Install net-tools (if not present) and check:
```
sudo apt install net-tools -y
sudo netstat -tnlp | grep 8080
```

You should see a Java process listening on 0.0.0.0:8080 or :::8080.

<img width="1266" height="217" alt="Image" src="https://github.com/user-attachments/assets/712ee63d-0bab-41dd-8aa8-82b8473a0d95" />


**🧭 Step 6: Access Jenkins Web UI**

Open your browser and navigate to:
```
http://<your-server-ip>:8080
```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/5c401549-7c01-4b15-bf42-9a99ae1481d0" />

**🔑 Step 7: Unlock Jenkins**

Retrieve the initial admin password:
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
<img width="1042" height="78" alt="Image" src="https://github.com/user-attachments/assets/595f3834-2c2c-4b8d-a982-e0eebd5e3df6" />

Copy the password and paste it into the Jenkins setup web page.



**🧩 Step 8: Install Suggested Plugins**

1. After logging in:

2. Click Install suggested plugins

3. Wait for plugin installation to complete (may take several minutes)



**👤 Step 9: Create First Admin User**

After plugins are installed:

1. Fill Username, Password, Full name, Email

2. Click Save and Continue

3. Click Start using Jenkins



**⚙️ Step 10: Configure Jenkins URL**

Set the Jenkins URL (Manage Jenkins → Configure System):
```
http://<your-server-ip>:8080/
```

Click Save.

✅ Jenkins setup is now complete!




**🧱 Create Your First Jenkins Job**
🎯 Goal

Run a simple job that writes a file to the workspace (verifies workspace + Jenkins functionality).

**Step 1: Create a New Job**

1. Go to Jenkins Dashboard

2. Click New Item

3. Enter name: First-Jenkins-Job

4. Choose Freestyle project

5. Click OK

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/17b299e7-4763-485e-b088-61526adac547" />



**Step 2: Configure the Job**

1. Add a short description in General (optional).

2. Under Build → Add build step → Execute shell

3. Paste this shell script:
```
#!/bin/sh
echo "This is my 1st-job" > jenkins.txt
ls -la
```

(Optional) Under Post-build Actions, add Archive the artifacts and set jenkins.txt to archive.

<img width="1919" height="721" alt="Image" src="https://github.com/user-attachments/assets/6621d879-6e56-4fe5-8d42-b3deb267b750" />

<img width="1913" height="897" alt="Image" src="https://github.com/user-attachments/assets/85c924c4-86f9-4875-ba3c-c16ff651b6a1" />

<img width="1326" height="637" alt="Image" src="https://github.com/user-attachments/assets/db4da7c1-e101-4448-b4e6-fcc5ba0d5734" />

**Step 3: Save and Build**

1. Click Save

2. Click Build Now

3. You will see a new build appear in Build History.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/171305d0-7a4a-456b-b492-638e68e7b151" />

<img width="526" height="721" alt="Image" src="https://github.com/user-attachments/assets/2903b59e-8b07-4b94-a50c-4ac1e04e9e27" />

<img width="1919" height="783" alt="Image" src="https://github.com/user-attachments/assets/1afbc9bf-57aa-4ef9-b8c7-2274f13744b1" />


**Step 4: View Console Output**

1. Click the build number (e.g., #1)

2. Click Console Output

Expected snippet:
```
This is my 1st-job
Finished: SUCCESS
```

<img width="1919" height="425" alt="Image" src="https://github.com/user-attachments/assets/b2160465-3390-4071-937e-3967b2f9bb93" />


**🧱 Verify Workspace Folder and Job Output (Server Side)**

When Jenkins runs a job it creates a workspace under /var/lib/jenkins/workspace/<job-name>.

1️⃣ Check Jenkins data directory
```
ls /var/lib/jenkins/
```

You should see directories: jobs nodes plugins secrets updates workspace

2️⃣ Check the workspace:
```
ls /var/lib/jenkins/workspace/
```

You should see:
```
First-Jenkins-Job
```

3️⃣ Inspect the workspace contents:
```
cd /var/lib/jenkins/workspace/First-Jenkins-Job
ls -la
cat jenkins.txt
```

You should see jenkins.txt containing:
```
This is my 1st-job
```
<img width="1806" height="362" alt="Image" src="https://github.com/user-attachments/assets/eaaddcb9-faac-4b13-a3e2-f5877e019133" />

**✅ Verification Complete**

Confirms:

Jenkins created the workspace directory

The job executed successfully and produced jenkins.txt

You can read build console and artifacts from server


**🧰 Troubleshooting**

Problem	Solution

1. Jenkins not accessible on port 8080	Check firewall: sudo ufw allow 8080
2. Jenkins failed to start	sudo journalctl -u jenkins --no-pager -n 200 and sudo systemctl restart jenkins
3. Java version error	Verify java --version (must be ≥ 11)
4. Plugin installation stuck	Restart Jenkins and check logs



**🧾 Conclusion**

You’ve successfully:

✅ Installed Jenkins on Ubuntu

✅ Configured Jenkins and unlocked the dashboard

✅ Created your first Hello World job

✅ Verified job output and workspace

🎉 Congratulations — Jenkins setup is complete!
