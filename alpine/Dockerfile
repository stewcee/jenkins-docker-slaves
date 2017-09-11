FROM alpine
MAINTAINER Steven Gardner <sgardner661@gmail.com>

ENV JAVA_HOME /usr/lib/jvm/java-1.8-openjdk
ENV PATH ${PATH}:${JAVA_HOME}/jre/bin:${JAVA_HOME}/bin

ENV JAVA_VERSION 8u131
ENV JAVA_ALPINE_VERSION 8.131.11-r2

RUN apk add --no-cache openjdk8="$JAVA_ALPINE_VERSION"

RUN apk --update add bash openssh git subversion curl maven && \
    rm -rf /var/cache/apk/* 

RUN /usr/bin/ssh-keygen -A

RUN addgroup jenkins && \
    adduser -D jenkins -s /bin/bash -G jenkins && \
    chown -R jenkins:jenkins /home/jenkins && \
    echo "jenkins:jenkins" | chpasswd

RUN mkdir  /home/jenkins/.ssh

COPY /files/authorized_keys /home/jenkins/.ssh/authorized_keys
COPY /files/config /home/jenkins/.ssh/config

RUN chown -R jenkins: /home/jenkins
RUN chmod 600 /home/jenkins/.ssh/authorized_keys
RUN chmod 644 /home/jenkins/.ssh/config
RUN chmod 700 /home/jenkins/.ssh

# Comment these lines to disable sudo
RUN apk --update add sudo && \
    rm -rf /var/cache/apk/* && \
    echo "%jenkins ALL=(ALL) ALL" >> /etc/sudoers

EXPOSE 22
CMD ["sudo", "/usr/sbin/sshd", "-D"]
