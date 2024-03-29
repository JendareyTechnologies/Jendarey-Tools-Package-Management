# Install Apache Tomcat on Ubuntu 22.04 - Using Manual method

|When ever you are installing an application in linux server, always install it in /opt - directory

- Prerequisites
- AWS Account
- Create ubuntu ec2 t2.medium Instance with 4GB of RAM.
- Security Group with required ports open (22, 8080)
- Java OpenJDK 11 and OpenJDK 17 install

# Change hostname to 'tomcat'
~~~
sudo hostnamectl set-hostname tomcat-server
sudo su - ubuntu
~~~

# Step 1: Update the server
~~~
sudo apt update  
~~~

# Step 2: Install required packages
~~~
cd /opt
~~~

~~~
sudo apt install wget unzip -y
~~~

~~~
sudo apt install openjdk-11-jdk openjdk-17-jdk -y
~~~
~~~
java -version
~~~

# Step 3: Download and extract Tomcat 10.1.18
~~~
sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.18/bin/apache-tomcat-10.1.18.zip
~~~

~~~
sudo unzip apache-tomcat-10.1.18.zip
~~~

~~~
sudo rm -rf apache-tomcat-10.1.18.zip
~~~

# Step 4: Rename Tomcat directory
~~~
sudo mv apache-tomcat-10.1.18 tomcat10
~~~

# Step 5: Set permissions and ownership
~~~
ls -l 
~~~

~~~
sudo chmod 777 -R /opt/tomcat10
~~~

~~~
sudo chown ubuntu:ubuntu -R /opt/tomcat10
~~~

# Step 6: Create symbolic links for managing Tomcat as a service
~~~
sudo ln -s /opt/tomcat10/bin/startup.sh /usr/bin/starttomcat
sudo ln -s /opt/tomcat10/bin/shutdown.sh /usr/bin/stoptomcat
~~~

# Step 7: Enable External Tomcat Management Access
~~~

sed -i '/<Valve className="org.apache.catalina.valves.RemoteAddrValve"/s/^/<!-- /' /opt/tomcat10/webapps/manager/META-INF/context.xml
sed -i '/allow="127\\.[0-9]\+\.[0-9]\+\.[0-9]\+\|::1\|0:0:0:0:0:0:0:1"/s/$/" --> /' /opt/tomcat10/webapps/manager/META-INF/context.xml

sed -i '/<Valve className="org.apache.catalina.valves.RemoteAddrValve"/s/^/<!-- /' /opt/tomcat10/webapps/host-manager/META-INF/context.xml
sed -i '/allow="127\\.[0-9]\+\.[0-9]\+\.[0-9]\+\|::1\|0:0:0:0:0:0:0:1"/s/$/" --> /' /opt/tomcat10/webapps/host-manager/META-INF/context.xml

~~~

# Step 8: Create Tomcat Users with Access Roles
~~~

sudo sed -i '/<\/tomcat-users>/i \<user username="admin" password="admin123" roles="manager-gui,admin-gui,manager-script"/>' /opt/tomcat10/conf/tomcat-users.xml
sudo sed -i '/<\/tomcat-users>/i \<user username="admin2" password="admin123" roles="admin-gui"/>' /opt/tomcat10/conf/tomcat-users.xml

~~~


# Step 8: Start Tomcat using the service links
~~~
starttomcat
~~~

# Step 9: Switch back to 'ubuntu' user
~~~
sudo su - ubuntu
~~~

- Open a web browser and go to http://localhost:8080 or publicIP:8080

# ============================================================


# Installing Tomcat using Scripts on ubuntu 22.04
==========================================================================


# vim tomcatsetup-script.sh
==========================================================================

~~~
#!/bin/bash

# Prerequisites
# - AWS Account
# - Create Ubuntu EC2 t2.medium Instance with 4GB of RAM.
# - Security Group with required ports open (22, 8080)
# - Java OpenJDK 11+ installed

# Change hostname to 'tomcat'
sudo hostnamectl set-hostname tomcatsetup-script

# Step 1: Update and upgrade Package List
sudo apt update

# Step 2: Change to /opt directory
cd /opt

# Step 3: Install required packages
sudo apt install wget unzip -y
sudo apt install openjdk-11-jdk openjdk-17-jdk -y

# Step 4: Download and extract Tomcat 10.1.18
sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.18/bin/apache-tomcat-10.1.18.zip
sudo unzip apache-tomcat-10.1.18.zip
sudo rm -rf apache-tomcat-10.1.18.zip

# Step 5: Rename Tomcat directory
sudo mv apache-tomcat-10.1.18 tomcat10

# Step 6: Set permissions and ownership
sudo chmod 777 -R /opt/tomcat10
sudo chown ubuntu:ubuntu -R /opt/tomcat10

# Step 7: Create symbolic links for managing Tomcat as a service
sudo ln -s /opt/tomcat10/bin/startup.sh /usr/bin/starttomcat
sudo ln -s /opt/tomcat10/bin/shutdown.sh /usr/bin/stoptomcat

# Step 8: Enable External Tomcat Management Access
sed -i '/<Valve className="org.apache.catalina.valves.RemoteAddrValve"/s/^/<!-- /' /opt/tomcat10/webapps/manager/META-INF/context.xml
sed -i '/allow="127\\.[0-9]\+\.[0-9]\+\.[0-9]\+\|::1\|0:0:0:0:0:0:0:1"/s/$/" --> /' /opt/tomcat10/webapps/manager/META-INF/context.xml

sed -i '/<Valve className="org.apache.catalina.valves.RemoteAddrValve"/s/^/<!-- /' /opt/tomcat10/webapps/host-manager/META-INF/context.xml
sed -i '/allow="127\\.[0-9]\+\.[0-9]\+\.[0-9]\+\|::1\|0:0:0:0:0:0:0:1"/s/$/" --> /' /opt/tomcat10/webapps/host-manager/META-INF/context.xml

# Step 9: Create Tomcat Users with Access Roles
sudo sed -i '/<\/tomcat-users>/i \<user username="admin" password="admin123" roles="manager-gui,admin-gui,manager-script"/>' /opt/tomcat10/conf/tomcat-users.xml
sudo sed -i '/<\/tomcat-users>/i \<user username="admin2" password="admin123" roles="admin-gui"/>' /opt/tomcat10/conf/tomcat-users.xml

# Step 10: Start Tomcat using the service links
starttomcat

# Step 11 : Switch back to 'ubuntu' user
sudo su - ubuntu
~~~



=====================================================


- sh tomcatsetup-script.sh

- or 

- chmod +x tomcatsetup-script.sh

- ./tomcatsetup-script.sh
