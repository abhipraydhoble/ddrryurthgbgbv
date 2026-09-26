# Docker, Docker Compose & Jenkins CI/CD

This README explains how to install **Docker, Docker Compose, and Jenkins**, and execute a simple Jenkins CI/CD pipeline.

---

# 1. Docker Installation

Copy and run all commands below:

```bash
sudo apt update -y
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ubuntu
newgrp docker
sudo chmod 777 /var/run/docker.sock
docker --version
docker ps
```

---

# 2. Docker Compose Installation

Copy and run all commands below:

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d'"' -f4)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
````
sudo chmod +x /usr/local/bin/docker-compose
````
```
docker-compose --version
```

---

# 3. Jenkins Installation

Copy and run all commands below:

```bash
sudo apt update
sudo apt install fontconfig openjdk-21-jre -y

java -version

sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key

echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install jenkins -y

sudo systemctl start jenkins
sudo systemctl enable jenkins

sudo systemctl status jenkins
```

---

# 4. Access Jenkins

Open Jenkins in your browser:

```text
http://SERVER-IP:8080
```

For AWS EC2, allow **TCP port 8080** in the Security Group.

Get the initial Jenkins password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

# 5. Give Jenkins Docker Permission

Copy and run:

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
sudo -u jenkins docker ps
sudo -u jenkins docker-compose --version
```

If both commands work, Jenkins can use Docker and Docker Compose.

---

# 6. Jenkins Pipeline

Create a file named:

```text
Jenkinsfile
```

Add:

```groovy
pipeline {
    agent any

    stages {

        stage("Code") {
            steps {
                git url: "https://github.com/LondheShubham153/django-notes-app.git",
                    branch: "main"
            }
        }

        stage("Deploy") {
            steps {
                sh "docker-compose up -d"
            }
        }
    }
}
```

---

# 7. Create Jenkins Pipeline Job

Go to Jenkins:

```text
New Item
```

Enter:

```text
django-notes-app
```

Select:

```text
Pipeline
```

Then configure:

```text
Pipeline
→ Pipeline script from SCM
→ SCM: Git
```

Repository:

```text
https://github.com/LondheShubham153/django-notes-app.git
```

Branch:

```text
*/main
```

Script Path:

```text
Jenkinsfile
```

Click **Save**.

---

# 8. Execute Pipeline

Click:

```text
Build Now
```

Jenkins will:

```text
GitHub
   ↓
Checkout Code
   ↓
Docker Compose
   ↓
docker-compose up -d
   ↓
Application Running
```

---

# 9. Verify Deployment

On the Jenkins server:

```bash
docker ps
```

Or:

```bash
docker-compose ps
```

Check application logs:

```bash
docker-compose logs
```

---

# 10. Docker Compose Commands

```bash
# Start containers
docker-compose up -d

# Stop and remove containers
docker-compose down

# Check containers
docker-compose ps

# View logs
docker-compose logs

# Follow logs
docker-compose logs -f

# Build images
docker-compose build
```

---

# Complete CI/CD Flow

```text
Developer
    ↓
GitHub
    ↓
Jenkins
    ↓
Checkout Code
    ↓
Docker Compose
    ↓
docker-compose up -d
    ↓
┌─────────┬─────────┬─────────┐
│  Nginx  │ Django  │  MySQL  │
│   :80   │  :8000  │  :3306  │
└─────────┴─────────┴─────────┘
    ↓
Application
```
