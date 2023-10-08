FROM ubuntu:latest
#update & install openssh server
RUN apt update && apt install \ 
    openssh-server -y \
    openssh-client -y \
    ca-certificates -y \
    sudo -y \
    curl -y \ 
    apt-transport-https -y \
    locales -y \
    gnupg2 -y \
    supervisor -y \
    openjdk-11-jdk -y \
    fontconfig -y \
    sshpass -y \
    git -y
#This is the Debian package repository of Jenkins to automate installation and upgrade. To use this repository, first add the key to your system:
RUN curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
#Then add a Jenkins apt repository entry:
RUN  echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
#https://www.google.com/linuxrepositories - server key to trust Jenkins repository
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 9B7D32F2D50582E6
#update container with the updated list of packages
RUN apt update -y
#Install Jenkins
RUN apt install jenkins -y
#Adds administrator user and the dedicated user directory
RUN useradd -rm -d /home/ubuntu -s /bin/bash -g root -G sudo -u 1000 administrator 
#Configures the following password for the user
RUN  echo 'administrator:password' | chpasswd
#Start the SSH service and expose port 22
RUN mkdir /root/.ssh
RUN service ssh start
EXPOSE 22 8080 50000
#Copy supervisoryd.conf configuration that defines the startup services
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
#start startup services when the server starts
CMD ["/usr/bin/supervisord"]