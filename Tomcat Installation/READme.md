# Install Apache Tomcat on Ubuntu 22.04 - Using Manual method

|When ever you are installing an application in linux server, always install it in /opt - directory

- Prerequisites
- AWS Account
- Create ubuntu ec2 t2.medium Instance with 4GB of RAM.
- Security Group with required ports open (22, 8080)
- Java OpenJDK 11+ installed

# Change hostname to 'tomcat'
~~~
sudo hostnamectl set-hostname tomcat
sudo su - ubuntu
~~~

# Step 1: Update and upgrade Package List
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

# Step 3: Download and extract Tomcat 10.1.13
~~~
sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.13/bin/apache-tomcat-10.1.13.zip
~~~

~~~
sudo unzip apache-tomcat-10.1.13.zip
~~~

~~~
sudo rm -rf apache-tomcat-10.1.13.zip
~~~

# Step 4: Rename Tomcat directory
~~~
sudo mv apache-tomcat-10.1.13 tomcat10
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
sudo hostnamectl set-hostname tomcatsetup-script

sudo apt update

cd /opt

sudo apt install wget unzip -y
sudo apt install openjdk-11-jdk openjdk-17-jdk -y

sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.13/bin/apache-tomcat-10.1.13.zip
sudo unzip apache-tomcat-10.1.13.zip
sudo rm -rf apache-tomcat-10.1.13.zip


sudo mv apache-tomcat-10.1.13 tomcat10

sudo chmod 777 -R /opt/tomcat10
sudo chown ubuntu:ubuntu -R /opt/tomcat10

sudo ln -s /opt/tomcat10/bin/startup.sh /usr/bin/starttomcat
sudo ln -s /opt/tomcat10/bin/shutdown.sh /usr/bin/stoptomcat


sed -i '/<Valve className="org.apache.catalina.valves.RemoteAddrValve"/s/^/<!-- /' /opt/tomcat10/webapps/manager/META-INF/context.xml
sed -i '/allow="127\\.[0-9]\+\.[0-9]\+\.[0-9]\+\|::1\|0:0:0:0:0:0:0:1"/s/$/" --> /' /opt/tomcat10/webapps/manager/META-INF/context.xml

sed -i '/<Valve className="org.apache.catalina.valves.RemoteAddrValve"/s/^/<!-- /' /opt/tomcat10/webapps/host-manager/META-INF/context.xml
sed -i '/allow="127\\.[0-9]\+\.[0-9]\+\.[0-9]\+\|::1\|0:0:0:0:0:0:0:1"/s/$/" --> /' /opt/tomcat10/webapps/host-manager/META-INF/context.xml


sudo sed -i '/<\/tomcat-users>/i \<user username="admin" password="admin123" roles="manager-gui,admin-gui,manager-script"/>' /opt/tomcat10/conf/tomcat-users.xml
sudo sed -i '/<\/tomcat-users>/i \<user username="admin2" password="admin123" roles="admin-gui"/>' /opt/tomcat10/conf/tomcat-users.xml


starttomcat


sudo su - ubuntu
~~~



=====================================================


- sh tomcatsetup-script.sh

- or 

- chmod +x tomcatsetup-script.sh

- ./tomcatsetup-script.sh
