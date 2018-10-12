FROM ubuntu:trusty
MAINTAINER Graylog, Inc. <hello@graylog.com>

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && \
    apt-get dist-upgrade -y && \
    apt-get install -y openssh-server sudo apt-utils software-properties-common apt-transport-https python-software-properties && \
    sed -i 's|session required pam_loginuid.so|session optional pam_loginuid.so|g' /etc/pam.d/sshd && \
    mkdir -p /var/run/sshd && \
    dpkg-reconfigure openssh-server && \
    groupadd -r jenkins && \
    useradd -m -r -g jenkins jenkins && \
    echo "jenkins:jenkins" | chpasswd && \
    echo "%jenkins ALL=NOPASSWD:ALL" > /etc/sudoers.d/jenkins && \
    chmod 0440 /etc/sudoers.d/jenkins && \
    usermod -a -G sudo jenkins && \
    mkdir /home/jenkins/workspace && \
    chown -R jenkins:jenkins /home/jenkins/workspace

RUN apt-get install -y --reinstall ca-certificates && \
    add-apt-repository "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EEA14886 && \
    apt-get update && \
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections && \
    apt-get install -y --force-yes oracle-java8-installer && \
    update-java-alternatives -s java-8-oracle

RUN apt-get update && \
    apt-get install -y wget curl unzip git build-essential autoconf clang libssl-dev && \
    git config --global https.postBuffer 1048576000 && \
    echo "export PATH=$PATH:/home/jenkins/depot_tools" >> /etc/bash.bashrc

RUN cd /home/jenkins && \
    git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git

VOLUME /home/jenkins/workspace

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]