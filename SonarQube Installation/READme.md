# Installing sonarQube
#!/bin/bash

# Prerequisites
- AWS Account
  
- EC2 T2.medium instance with Red Hat OS (4GB RAM)
  
- Open required ports in Security Group (e.g., 9000)
  
- Attach Security Group to EC2 instance

# 1. Create Sonar User

```bash

sudo yum update -y

```

```bash

sudo useradd sonar

```

```bash

echo "sonar ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/sonar

```

```bash

sudo hostnamectl set-hostname sonar
sudo su - ec2-user

```

# Assign a password to sonar user

```bash
echo "sonar:admin" | sudo chpasswd
```

# 2. Enable Password Authentication for SSH

```bash
sudo sed -i "/^[^#]*PasswordAuthentication[[:space:]]no/c\PasswordAuthentication yes" /etc/ssh/sshd_config
sudo service sshd restart
```

# 3. Install Java JDK 17 and other dependencies

```bash
sudo yum install unzip wget git java-17-openjdk-devel -y
```

# 4. Download and Extract SonarQube Server Software

```bash
cd /opt

```

```bash

sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.2.1.78527.zip

```

```bash
sudo unzip sonarqube-10.2.1.78527.zip

```

```bash

sudo rm -rf sonarqube-10.2.1.78527.zip

```

```bash

sudo mv sonarqube-10.2.1.78527 sonarqube

```
# 5. Grant Permissions to Sonar User

```bash
sudo chown -R sonar:sonar /opt/sonarqube/
sudo chmod -R 775 /opt/sonarqube/
```

# 6. Start SonarQube Server

```bash
sudo su - sonar -c "/opt/sonarqube/bin/linux-x86-64/sonar.sh start"
sudo su - sonar -c "/opt/sonarqube/bin/linux-x86-64/sonar.sh status"
```

# 7. Access SonarQube

- SonarQube's default port is 9000. Access it via a web browser.
  
- Replace <your_public_ip> with your EC2 instance's public IP.
  
- Example: http://<your_public_ip>:9000
  
- Alternatively, use curl to verify:
  
- curl -v localhost:9000

```bash
https://devopscube.com/setup-and-configure-sonarqube-on-linux/
```

# Default credentials:
- Username: admin
  
- Password: admin

