# Getting Started with Jenkins:

- Installation and Setup:
- Installing Jenkins on Ubuntu
- This script works on Ubuntu OS
- Create a Server on AWS and your server must have at least 4GB of RAM

# Set the hostname and switch to the root user
```bash
sudo hostnamectl set-hostname jenkins-scripts
sudo su - ubuntu
sudo timedatectl set-timezone America/Chicago
```


# Install necessary packages
```bash
sudo apt-get update
sudo apt-get install tree git wget openjdk-17-jdk -y
```

# Add Jenkins repository
```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
```

```bash
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

```bash
sudo apt-get update 
```

```bash
sudo apt-get install jenkins -y
```


# Reload systemd manager configuration

```bash
sudo systemctl daemon-reload
```


# Enable and start Jenkins service

```bash
sudo systemctl enable jenkins
```

```bash
sudo systemctl start jenkins
```

```bash
sudo systemctl status jenkins
```

