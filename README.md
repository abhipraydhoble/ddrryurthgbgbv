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



# . Jenkins Pipeline


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


