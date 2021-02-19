FROM jenkins/jenkins:lts

ARG HOST_UID
ARG HOST_GID

USER root

RUN apt-get -y update
RUN apt-get -y install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
RUN add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
RUN apt-get update
RUN apt-get -y install docker-ce docker-ce-cli containerd.io

RUN usermod -u $HOST_UID jenkins
RUN groupmod -g $HOST_GID docker
RUN usermod -aG docker jenkins

ENV JENKINS_SLAVE_AGENT_PORT=50000

COPY resources/plugins/plugins.txt /usr/share/jenkins/ref/
RUN /usr/local/bin/install-plugins.sh $(cat /usr/share/jenkins/ref/plugins.txt)

COPY resources/users/*.groovy /usr/share/jenkins/ref/init.groovy.d/

USER jenkins
