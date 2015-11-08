FROM hypriot/rpi-java

RUN apt-get update && apt-get install -y wget git

ENV JENKINS_HOME /var/jenkins_home
ENV JENKINS_SLAVE_AGENT_PORT 50000

# Jenkins is ran with user `jenkins`, uid = 1000
# If you bind mount a volume from host/volume from a data container,
# ensure you use same uid
RUN useradd -d "$JENKINS_HOME" -u 1000 -m -s /bin/bash jenkins

# Jenkins home directoy is a volume, so configuration and build history
# can be persisted and survive image upgrades
VOLUME /var/jenkins_home

RUN chown -R jenkins "$JENKINS_HOME"

# for main web interface:
EXPOSE 8080

# will be used by attached slave agents:
EXPOSE 50000


RUN mkdir -p /opt/jenkins && \
        cd /opt/jenkins && \
        wget http://mirrors.jenkins-ci.org/war/latest/jenkins.war

CMD java -jar /opt/jenkins/jenkins.war --prefix=$PREFIX