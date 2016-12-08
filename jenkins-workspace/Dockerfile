FROM centos:7
MAINTAINER Krasilnikov Dmitry

# Create the jenkins user
RUN useradd -d "/home/jenkins" -u 1000 -m -s /bin/bash jenkins

# Create the folders and volume mount points
RUN mkdir -p /home/jenkins/workspace
RUN chown -R jenkins:jenkins /home/jenkins/workspace
VOLUME ["/home/jenkins/workspace"]

USER jenkins
CMD ["echo", "Data container for Jenkins Slave Workspace"]