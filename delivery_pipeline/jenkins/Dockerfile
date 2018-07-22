FROM jenkins/jenkins:lts
USER root
RUN apt-get update && \
apt-get -y install apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     maven \
     dnsutils \
     software-properties-common && \
apt-get update 
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

COPY udclient.zip /opt/
RUN unzip /opt/udclient.zip -d /opt/udclient && \
    chown -R jenkins:jenkins /opt && chmod +x /opt/udclient/udclient/udclient

USER jenkins
EXPOSE 8080
ENV JENKINS_OPTS --prefix=/jenkins

