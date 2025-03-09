# handson-001
Install jenkins 
https://www.jenkins.io/doc/book/installing/linux/#debianubuntu

step one :Installation of Java
Jenkins requires Java to run, yet not all Linux distributions include Java by default. Additionally, not all Java versions are compatible with Jenkins.

There are multiple Java implementations which you can use. OpenJDK is the most popular one at the moment, we will use it in this guide.

Update the Debian apt repositories, install OpenJDK 17, and check the installation with the commands:

sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version
openjdk version "17.0.13" 2024-10-15
OpenJDK Runtime Environment (build 17.0.13+11-Debian-2)
OpenJDK 64-Bit Server VM (build 17.0.13+11-Debian-2, mixed mode, sharing)

Step 2: 
Debian/Ubuntu
On Debian and Debian-based distributions like Ubuntu you can install Jenkins through apt.

Long Term Support release
A LTS (Long-Term Support) release is chosen every 12 weeks from the stream of regular releases as the stable release for that time period. It can be installed from the debian-stable apt repository.

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins

Ste3
Start Jenkins
You can enable the Jenkins service to start at boot with the command:

sudo systemctl enable jenkins
You can start the Jenkins service with the command:

sudo systemctl start jenkins
You can check the status of the Jenkins service using the command:

sudo systemctl status jenkins
If everything has been set up correctly, you should see an output like this:

Loaded: loaded (/lib/systemd/system/jenkins.service; enabled; vendor preset: enabled)
Active: active (running) since Tue 2018-11-13 16:19:01 +03; 4min 57s ago


Insteall docker 
https://docs.docker.com/engine/install/ubuntu/

Installation methods
You can install Docker Engine in different ways, depending on your needs:

Docker Engine comes bundled with Docker Desktop for Linux. This is the easiest and quickest way to get started.

Set up and install Docker Engine from Docker's apt repository.

Install it manually and manage upgrades manually.

Use a convenience script. Only recommended for testing and development environments.

Install using the apt repository
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker apt repository. Afterward, you can install and update Docker from the repository.

Set up Docker's apt repository.


# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
Install the Docker packages.

Latest Specific version
To install the latest version, run:


 sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
Verify that the installation is successful by running the hello-world image:


 sudo docker run hello-world



 # Install sonarqube 

 use the documentation , below is the link 
 https://www.howtoforge.com/how-to-install-sonarqube-on-ubuntu-22-04/#google_vignette

 follow these commands
 sudo apt update
 sudo apt install default-jdk
 java -version
 wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -
 #if the out put is deprecated rund this 
 wget -qO- https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo tee /etc/apt/trusted.gpg.d/postgresql.asc > /dev/null
# then 
sudo apt update

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'

sudo apt update

sudo apt install postgresql-13
sudo systemctl is-enabled postgresql
sudo systemctl status postgresql

# in case you have warning like this is configured multiple times in /etc/apt/sources.list.d/pgdg.list:1 and /etc/apt/sources.list.d/pgdg.list:2, It looks like there are a few issues:

# Duplicate Entries in pgdg.list
# You have accidentally added the PostgreSQL repository multiple times. This is causing warnings. 

# Package Not Found
# Even after adding the repository, postgresql-13 is not being located, possibly due to incorrect repository information or missing an apt update.

# Fix Steps:
1. Remove the duplicate repository entries, Run the following command to clean up the repository list:

# sudo rm /etc/apt/sources.list.d/pgdg.list


2. Add the PostgreSQL Repository Properly
Re-add the correct repository using:

# echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" | sudo tee /etc/apt/sources.list.d/pgdg.list
3. Add the GPG Key Properly
Use this method (since apt-key is deprecated):

wget -qO- https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo tee /etc/apt/trusted.gpg.d/postgresql.asc > /dev/null
4. Update the Package List

sudo apt update
5. Install PostgreSQL 13

sudo apt install -y postgresql-13
Now, check if PostgreSQL is installed:

# psql --version

sudo systemctl is-enabled postgresql
sudo systemctl status postgresql

sudo -u postgres psql
CREATE USER sonarqube WITH PASSWORD 'Password';
CREATE DATABASE sonarqube OWNER sonarqube;
GRANT ALL PRIVILEGES ON DATABASE sonarqube TO sonarqube;
\l
\du
\q

Setting up System

sudo useradd -b /opt/sonarqube -s /bin/bash sonarqube
sudo vim /etc/sysctl.conf
# Add the following configuration to the bottom of the line. The SonarQube required the kernel parameter vm.max_map_count to be greater than '524288' and the fx.file-max to be greater than '131072'.
vm.max_map_count=524288
fs.file-max=131072
:wq
sudo sysctl --system

ulimit -n 131072
ulimit -u 8192

sudo vim /etc/security/limits.d/99-sonarqube.conf
sonarqube   -   nofile   131072
sonarqube   -   nproc    8192
# Save the file and close the editor when you are finished.

# Downloading SonarQube Package

sudo apt install unzip software-properties-common wget
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.6.1.59531.zip
unzip sonarqube-9.6.1.59531.zip
ls
rm -rf sonarqube-9.6.1.59531.zip
mv sonarqube-9.6.1.59531 /opt/sonarqube
sudo chown -R sonarqube:sonarqube /opt/sonarqube
# to see the output run 
ls /opt/sonarqube 
/opt/sonarqube

# Configuring SonarQube
vim /opt/sonarqube/conf/sonar.properties

# For the database configuration, uncomment some of the following options and change the default value using your database details.

sonar.jdbc.username=sonarqube
sonar.jdbc.password=Password
# in the file the database reference is oracle and in the command below is postgresql. thus we replaced the oracle sonar.jdbc.url value with the postgresql value
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube

# uncomment the following lines
sonar.search.javaOpts=-Xmx512m -Xms512m -XX:MaxDirectMemorySize=256m -XX:+HeapDumpOnOutOfMemoryError

Lastly, uncomment and change the following configurations to set up the IP address and port of the SonarQube will be running. Also, the log level will be 'INFO" and stored in the 'logs' directory of the SonarQube installation directory.

sonar.web.host= (replace the 0.0.0.0 with the IP address of your VM)
sonar.web.port=9000
# add the value -server to the sonar.web.javaAdditionalOpts
sonar.web.javaAdditionalOpts=-server

sonar.log.level=INFO
sonar.path.logs=logs


vim /etc/systemd/system/sonarqube.service
# Add the following configuration to the file

[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking
ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop
User=sonarqube
Group=sonarqube
Restart=always
LimitNOFILE=65536
LimitNPROC=4096

[Install]
WantedBy=multi-user.target

# Save the file and exit the editor when you are done.

# Now, reload the systemd manager by using the following command.
sudo  systemctl daemon-reload
# After that, start and enable the 'sonarqube.service' via the systemctl command below.

sudo systemctl start sonarqube.service
sudo systemctl enable sonarqube.service
# Lastly, verify the 'sonarqube.service' status using the following command and make sure its status is running.
sudo systemctl status sonarqube.service

# Running SonarQube with Reverse Proxy
sudo apt install nginx

sudo systemctl is-enabled nginx
sudo systemctl status nginx
# After you have the Nginx web server is running, you will create a new server block configuration that will be used as a reverse proxy for SonarQube. Create a new server blocks configuration '/etc/nginx/sites-available/sonarqube.conf' using the following command.
vim /etc/nginx/sites-available/sonarqube.conf

# Replace with your server IP address in line 269
server {

    listen 80;
    server_name sonar.howtoforge.local;
    access_log /var/log/nginx/sonar.access.log;
    error_log /var/log/nginx/sonar.error.log;
    proxy_buffers 16 64k;
    proxy_buffer_size 128k;

    location / {
        proxy_pass http://52.55.225.149:9000; 
        proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto http;
    }
}

# Save the file and exit the editor when you are finished. Next, activate the server block configuration 'sonarqube.conf' by creating a symlink of that file to the '/etc/nginx/sites-enabled' directory. Then, verify your Nginx configuration files.
sudo ln -s /etc/nginx/sites-available/sonarqube.conf /etc/nginx/sites-enabled/
sudo nginx -t

sudo systemctl restart nginx

# SonarQube Installatioon

