🧪 Testing a JavaScript (Angular) Application using SonarQube on Ubuntu

This document explains how to analyze and test a JavaScript/Angular project using SonarQube for static code analysis on an Ubuntu server.

🧰 1. Prerequisites
Ensure your Ubuntu server has the following tools installed:
```
sudo apt update
sudo apt install -y openjdk-17-jdk nodejs npm unzip wget
```

Verify installations:
```
java -version
node -v
npm -v
```


🚀 2. Clone the JavaScript / Angular Project
Clone the target repository you want to test.
Here we use the AngularCalculator app as an example:

```
git clone https://github.com/Ai-TechNov/AngularCalculator.git
cd AngularCalculator
ls
```


⚙️ 3. Install Node.js Dependencies
Install all required dependencies for the project:

```
npm install
```

Check Node.js and npm versions to confirm setup:

```
node -v
npm -v
```


🧩 4. Fix OpenSSL Build Error (for Node 17+)
If you face an error like
Error: error:0308010C:digital envelope routines::unsupported
during build, enable the OpenSSL legacy provider:

```
export NODE_OPTIONS=--openssl-legacy-provider
```

Now build the Angular project:

```
npm run build
```



🧠 5. Set Up SonarQube Server
Download and extract SonarQube (version 6.7.7 used here):

```
cd ~
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-6.7.7.zip
sudo apt install unzip -y
unzip sonarqube-6.7.7.zip
```

Start the SonarQube server:

```
cd sonarqube-6.7.7/bin/linux-x86-64/
./sonar.sh start
./sonar.sh status
```

Access SonarQube in your browser:

```
http://<your-server-ip>:9000
```

Default credentials:
Username: admin
Password: admin
    🔑 After login, create a new Project and generate a Token (used for authentication during scan).



📄 6. Create sonar-project.properties File
Inside your AngularCalculator directory, create the configuration file:

```
vi sonar-project.properties
```

Paste the following content (update your own values):

```
sonar.projectKey=js-test
sonar.projectName=js-test
sonar.projectVersion=1.0
sonar.sourceEncoding=UTF-8
sonar.sources=src
sonar.host.url=http://13.57.28.112:9000
sonar.login=44e866a02733b6cdfe0bf7cf125bde49b0374f82
```



🧮 7. Install Sonar Scanner
Install the Sonar Scanner CLI globally using npm:

```
sudo npm install -g sonar-scanner
```

Verify installation:

```
sonar-scanner -v
```



🔍 8. Run SonarQube Scan
From inside your project directory:

```
sonar-scanner
```

Then open your browser and navigate to:

```
http://13.57.28.112:9000/dashboard?id=js-test
```



🧾 9. Expected Output
After the scan completes, you’ll get a SonarQube dashboard showing:
    Code smells 🧩
    Bugs 🐛
    Vulnerabilities 🔐
    Maintainability metrics ⚙️
    Coverage and duplication 📊



✅ 10. Summary
| Step | Task                            | Command                                                         |
| ---- | ------------------------------- | --------------------------------------------------------------- |
| 1    | Install prerequisites           | `sudo apt install -y openjdk-17-jdk nodejs npm`                 |
| 2    | Clone project                   | `git clone https://github.com/Ai-TechNov/AngularCalculator.git` |
| 3    | Install dependencies            | `npm install`                                                   |
| 4    | Fix OpenSSL issue               | `export NODE_OPTIONS=--openssl-legacy-provider`                 |
| 5    | Build project                   | `npm run build`                                                 |
| 6    | Start SonarQube                 | `./sonar.sh start`                                              |
| 7    | Create sonar-project.properties | Contains project & server details                               |
| 8    | Install sonar-scanner           | `sudo npm install -g sonar-scanner`                             |
| 9    | Run scan                        | `sonar-scanner`                                                 |



🧠 Notes
    Use Java 11 or 17 for compatibility.

    Ensure the SonarQube server is running before scanning.

    Always create the file sonar-project.properties in the root of your project.

    For large projects, adjust memory in /usr/local/lib/node_modules/sonar-scanner/conf/sonar-scanner.properties.
