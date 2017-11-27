FROM jenkins/jenkins:lts
MAINTAINER aelmallem

# Expose Ports for web and slave agents
EXPOSE 8080
EXPOSE 50000
USER root
# Copy in local config filesfiles
COPY init.groovy /usr/share/jenkins/ref/init.groovy.d/tcp-slave-agent-port.groovy
COPY jenkins.sh /usr/local/bin/jenkins.sh
COPY plugins.sh /usr/local/bin/plugins.sh
RUN chmod +x /usr/local/bin/plugins.sh
RUN chmod +x /usr/local/bin/jenkins.sh

COPY plugins.txt /tmp/plugins.txt
RUN /usr/local/bin/plugins.sh /tmp/plugins.txt

# Copy in the Docker certs, we'll use /usr/local/etc for them
# Certs should must be added in this order on jenkins docker plugin or yadp
COPY certs/key.pem /usr/local/etc/jenkins/certs/key.pem
COPY certs/cert.pem /usr/local/etc/jenkins/certs/cert.pem
COPY certs/ca.pem /usr/local/etc/jenkins/certs/ca.pem


# Make sure cert permissions are set correctly
RUN chmod +r /usr/local/etc/jenkins/certs/ca.pem
RUN chmod +r /usr/local/etc/jenkins/certs/cert.pem
RUN chown -R jenkins:jenkins /usr/local/etc/jenkins

# Switch to the jenkins user
USER jenkins

# Tini as the entry point to manage zombie processes
ENTRYPOINT ["/bin/tini", "--", "/usr/local/bin/jenkins.sh"]