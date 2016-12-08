FROM centos:7
MAINTAINER Maxfield Stewart

RUN echo $'[dockerrepo] \n\
name            = Docker Repository \n\
baseurl         = https://yum.dockerproject.org/repo/main/centos/7/ \n\
enabled         = 1 \n\
gpgcheck        = 1 \n\
gpgkey=https://yum.dockerproject.org/gpg' > /etc/yum.repos.d/docker.repo

# Install Essentials
RUN yum update -y && \
    yum clean all

# Install Packages
RUN yum install -y git && \
    yum install -y wget && \
	yum install -y openssh-server && \
	yum install -y java-1.8.0-openjdk && \
	yum install -y docker-engine && \
	yum install -y sudo && \
	yum clean all

# gen dummy keys, centos doesn't autogen them like ubuntu does
RUN /usr/bin/ssh-keygen -A

# Set SSH Configuration to allow remote logins without /proc write access
RUN sed -ri 's/^session\s+required\s+pam_loginuid.so$/session optional pam_loginuid.so/' /etc/pam.d/sshd

# Create Jenkins User
RUN useradd jenkins -m -s /bin/bash
RUN usermod -aG docker jenkins

# Add public key for Jenkins login
RUN mkdir /home/jenkins/.ssh
COPY /files/authorized_keys /home/jenkins/.ssh/authorized_keys

RUN chown -R jenkins /home/jenkins
RUN chgrp -R jenkins /home/jenkins

RUN chmod 600 /home/jenkins/.ssh/authorized_keys
RUN chmod 700 /home/jenkins/.ssh

# Add the jenkins user to sudoers
RUN echo "jenkins    ALL=(ALL)    ALL" >> etc/sudoers

# Copy in the Docker certs, we'll use /usr/local/etc for them
COPY certs/ca.pem /home/jenkins/.docker/ca.pem
COPY certs/cert.pem /home/jenkins/.docker/cert.pem
COPY certs/key.pem /home/jenkins/.docker/key.pem

RUN chmod +r /home/jenkins/.docker/ca.pem
RUN chmod +r /home/jenkins/.docker/cert.pem
RUN chmod +r /home/jenkins/.docker/key.pem

RUN chown -R jenkins:jenkins /home/jenkins/.docker

# Set Name Servers
COPY /files/resolv.conf /etc/resolv.conf

VOLUME ["/home/jenkins/workspace"]

ENV DOCKER_HOST tcp://172.17.0.1:4243
ENV DOCKER_TLS_VERIFY 1
ENV DOCKER_CERT_PATH /home/jenkins/.docker

# Expose SSH port and run SSHD
EXPOSE 22
CMD ["/usr/sbin/sshd","-D"]
