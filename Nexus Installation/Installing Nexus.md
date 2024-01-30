# Nexus Installation:
---------------------
- Install and start Nexus as a service
  
- This script works on Ubuntu OS
  
- Your server must have at least 4GB of RAM
  
# Become the root / admin user via: sudo su -

# 1. Create a Nexus user to manage Nexus and also set hostname

```bash
sudo hostnamectl set-hostname nexus
sudo adduser nexus
```

# 2. Give sudo access to the Nexus user and Switch to the Nexus user
```bash
echo "nexus ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/nexus
sudo su - nexus
```

# 3. Enable Password Authentication for SSH
```bash
sudo sed -i "/^[^#]*PasswordAuthentication[[:space:]]no/c\PasswordAuthentication yes" /etc/ssh/sshd_config
sudo service sshd restart
```

# 4. Install prerequisites: Java, git, unzip
```bash
cd /opt

```

```bash

sudo apt-get update

```

```bash

sudo apt-get install wget git unzip -y

```

```bash

sudo apt-get install openjdk-8-jdk openjdk-11-jdk openjdk-17-jdk -y

```
# 5. Download Nexus software and extract it
```bash
sudo wget https://download.sonatype.com/nexus/3/nexus-3.61.0-02-unix.tar.gz

```

```bash

sudo tar -zxvf nexus-3.61.0-02-unix.tar.gz

```

```bash

sudo rm -rf nexus-3.61.0-02-unix.tar.gz

```

```bash

sudo mv /opt/nexus-3.61.0-02 /opt/nexus

```
# 6. Change owner and group permissions for directories
```bash

sudo chown -R nexus:nexus /opt/nexus
sudo chown -R nexus:nexus /opt/sonatype-work
sudo chmod -R 775 /opt/nexus
sudo chmod -R 775 /opt/sonatype-work

```

# 7. Open /opt/nexus/bin/nexus.rc and configure Nexus to run as the Nexus user
```bash
sudo sed -i 's/#run_as_user=""/run_as_user="nexus"/' /opt/nexus/bin/nexus.rc
```
- or use
- 
```bash
echo  'run_as_user="nexus" ' > /opt/nexus/bin/nexus.rc
```

# 8. Configure Nexus to run as a service
```bash
sudo ln -s /opt/nexus/bin/nexus /etc/init.d/nexus
```

# 9. Enable and start the Nexus service

```bash
sudo systemctl daemon-reload

sudo systemctl enable nexus

sudo systemctl start nexus

sudo systemctl status nexus

```

```bash
echo "End of Nexus installation"
```
====================================================================================
====================================================================================
http://<your_server_ip>8081/jendarey 

default nexus login:
userName: = admin 
password = to get password:  -  cat /opt/sonatype-work/nexus3/admin.password

