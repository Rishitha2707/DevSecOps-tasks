# Deploying a Java Maven Webapp to JBoss (Two-Server Setup)

This guide demonstrates how to **build a Java web application on a build server using Maven** and then **deploy it to a production server running JBoss / WildFly**. It reflects a real-world DevOps setup where building and deployment occur on separate machines.

---

## 🧩 Architecture Overview

**Workflow:**
```
Developer → GitHub → Build Server (Maven) → WAR Artifact → Production Server (JBoss/WildFly)
```

### Servers

| Server Role | Purpose | Example Hostname | Key Software |
|--------------|----------|------------------|---------------|
| Build Server | Build and package the Java app | `build-server` | Java JDK 11+, Apache Maven 3.8.6+ |
| Production Server | Host and deploy the WAR | `prod-server` | JBoss EAP 7.4 / WildFly 26+ (Java EE 8 Compatible) |

---

## ⚙️ Compatible Versions

| Component | Recommended Version | Notes |
|------------|----------------------|--------|
| Java | 11 or 17 | Compatible with Maven and JBoss/WildFly |
| Apache Maven | 3.8.6+ | Required to build the WAR file |
| JBoss EAP | 7.4 | Stable enterprise release (supports Jakarta EE 8) |
| WildFly | 26 or newer | Community version of JBoss EAP |
| OS | Ubuntu 22.04 / Amazon Linux 2 | Both work fine for this setup |

---

## 🖥️ Step 1: Clone and Build on the Build Server

### 1. Clone a Java Web Application
You can use the sample project below or your own:

```bash
git clone https://github.com/DEV3L/mvn-hello-world-web-app.git
cd mvn-hello-world-web-app
```

### 2. Build the WAR using Maven
```bash
mvn clean package -DskipTests
```

After a successful build, the WAR will be created in the `target/` directory:
```
target/mvn-hello-world-web-app.war
```

---

## 📦 Step 2: Transfer WAR to Production Server

Once the WAR file is built, transfer it from the build server to the production server.

### Using SCP (Secure Copy)
```bash
scp -i /path/to/key.pem target/mvn-hello-world-web-app.war \
    ubuntu@<prod-server-ip>:/home/ubuntu/
```

This securely copies the WAR to the production server’s home directory.

---

## 🚀 Step 3: Deploy on the Production Server

SSH into the production server:
```bash
ssh -i /path/to/key.pem ubuntu@<prod-server-ip>
```

### Move WAR to the JBoss Deployments Directory
```bash
sudo mv /home/ubuntu/mvn-hello-world-web-app.war /opt/wildfly/standalone/deployments/
```

WildFly automatically detects the WAR and begins deployment.

Alternatively, deploy via the CLI:
```bash
cd /opt/wildfly/bin
./jboss-cli.sh --connect --command="deploy /opt/wildfly/standalone/deployments/mvn-hello-world-web-app.war --force"
```

### Verify Deployment
```bash
./jboss-cli.sh --connect --command="deployment-info"
```
Expected output:
```
NAME                          RUNTIME-NAME                   STATUS
mvn-hello-world-web-app.war   mvn-hello-world-web-app.war    OK
```

Then open:
👉 `http://<prod-server-ip>:8080/mvn-hello-world-web-app/`

---

## 🧾 Configuration Notes

### Example `pom.xml` Configuration
Ensure your project is packaged as a WAR:
```xml
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example</groupId>
  <artifactId>mvn-hello-world-web-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>
</project>
```

### Optional: Define Context Root (jboss-web.xml)
If you want a specific context path, create:
`src/main/webapp/WEB-INF/jboss-web.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jboss-web>
  <context-root>/myapp</context-root>
</jboss-web>
```

Then access your app at: `http://<prod-server-ip>:8080/myapp/`

---

## 🧠 Troubleshooting Tips

| Issue | Cause | Fix |
|--------|--------|-----|
| `WFLYSRV0059` Deployment failed | Wrong Java version or dependency mismatch | Ensure Java 11+ and EE 8-compatible libraries |
| Port 8080 not accessible | Firewall or port conflict | Allow inbound on port 8080 or change port in `standalone.xml` |
| WAR not deploying | Incorrect path or permissions | Ensure WAR is placed in `/standalone/deployments` and owned by `wildfly` user |

---

## ✅ Verification Commands
```bash
# Check JBoss status
sudo systemctl status wildfly

# List deployed apps
/opt/wildfly/bin/jboss-cli.sh --connect --command="deployment-info"

# View logs
tail -f /opt/wildfly/standalone/log/server.log
```

If you see `WFLYSRV0025: JBoss EAP/WildFly started`, the deployment succeeded.

---

## 🧰 Optional Enhancements
- Set up **artifact repository (Nexus / Artifactory)** for storing WAR files.
- Automate file transfer using **cron or shell scripts**.
- Integrate a **reverse proxy (Nginx)** for routing to port 80.

---

### ✅ Summary
- Maven builds and packages the WAR on the build server.
- WAR is transferred securely to the production server.
- JBoss/WildFly automatically deploys the app.
- Works with Java 11+, Maven 3.8+, and WildFly 26+/JBoss 7.4+.

This setup reflects a clean **two-server deployment architecture** — fast, modular, and production-ready.

