FROM centos:7
MAINTAINER Steven Gardner <sgardner661@gmail.com>

ENV SBT_VERSION 1.0.0
ENV JDK_VERSION 1.8.0

RUN yum update -y && \
	yum clean all

RUN yum install -y git && \
	yum install -y svn && \
	yum install -y wget && \
	yum install -y maven && \
	yum install -y openssh-server && \
	yum install -y java-$JDK_VERSION-openjdk && \
	yum install -y sudo && \
	yum localinstall -y http://dl.bintray.com/sbt/rpm/sbt-$SBT_VERSION.rpm && \
	yum clean all

RUN /usr/bin/ssh-keygen -A

RUN sed -ri 's/^session\s+required\s+pam_loginuid.so$/session optional pamloginuid.so/' /etc/pam.d/sshd

RUN useradd jenkins -m -s /bin/bash

RUN mkdir /home/jenkins/.ssh

COPY /files/authorized_keys /home/jenkins/.ssh/authorized_keys

RUN chown -R jenkins /home/jenkins && \
    chgrp -R jenkins /home/jenkins && \
    chmod 600 /home/jenkins/.ssh/authorized_keys && \
    chmod 700 /home/jenkins/.ssh

RUN echo "jenkins ALL=(ALL) ALL" >> /etc/sudoers

COPY /files/resolv.conf /etc/resolv.conf
COPY /files/known_hosts /home/jenkins/.ssh/known_hosts

EXPOSE 22
CMD ["/usr/sbin/sshd","-D"]
