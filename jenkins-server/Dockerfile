FROM jenkins/jenkins:lts

#ENV http_proxy http://forwardproxy:3128
#ENV https_proxy http://forwardproxy:3128
#ENV no_proxy localhost,169.254.169.254,patching-server-hui.ext.national.com.au,artifactory.aus.thenational.com
ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false -Dpermissive-script-security.enabled=true"

# New plugins must be placed in the plugins file
ADD plugins.txt /usr/share/jenkins/ref/plugins.txt

# Create admin user
ADD scripts/security.groovy /usr/share/jenkins/ref/init.groovy.d/security.groovy

# Copy scriptApprovals required for Groovy scripts to be allowed to be executed
ADD scriptApprovals/scriptApproval.xml /var/jenkins_home/scriptApproval.xml

USER root
RUN xargs /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

# Copy plugins which have been blacklisted by Jenkins
COPY plugins/*.hpi /usr/share/jenkins/ref/plugins/

# Create a link between /usr/bin/git and /usr/loca/bin/git so jenkins knows where git is
RUN ln -sf /usr/bin/git /usr/local/bin/git

# Configure Jenkins slave port to be 8085
ENV JENKINS_SLAVE_AGENT_PORT 8085

# Setting the Jenkins security model to "Private" This should allow us to set permissions in Jenkins on a per user basis
ENV JENKINS_SECURITY_REALM HudsonPrivateSecurityRealm

# Make jenkins' user new home directory
RUN mkdir /var/jenkins

# Set jenkins' user home directory
RUN usermod -d /var/jenkins jenkins

# Make ssh dir
RUN mkdir -p /var/jenkins/.ssh/

# Copy over private key, and set permissions
# This folder is not in the repo. You will have to copy this from an existing jenkins master or from the ami.
#ADD sshkeys/id_rsa /var/jenkins/.ssh/id_rsa

# Create known_hosts
RUN touch /var/jenkins/.ssh/known_hosts

# Create the jenkins_home folder for jenkins to put it's files in
# RUN mkdir /var/jenkins_home

# Add jenkins' user permissions to it's home directory
RUN chown -R jenkins:jenkins /var/jenkins/

# Add jenkins' user permissions to the jenkins home directoroy
RUN chown -R jenkins:jenkins /var/jenkins_home

# Install Nginx.

#RUN \
#  apt-get update -y && \
#  apt-get install -y software-properties-common && \
#  add-apt-repository ppa:nginx/stable && \
#  apt-get install -y nginx && \
#  rm -rf /var/lib/apt/lists/* && \
#  echo "\ndaemon off;" >> /etc/nginx/nginx.conf && \
#  chown -R www-data:www-data /var/lib/nginx

# Define mountable directories.
#VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx", "/var/www/html"]

# Define working directory.
#WORKDIR /etc/nginx

#ADD conf/nginx.conf /etc/nginx/nginx.conf
#ADD conf/jenkins.conf /etc/nginx/conf.d/jenkins.conf

# Define default command.
#CMD ["nginx"]

# Expose ports.
#EXPOSE 80
#EXPOSE 443


# install packages

#RUN apt-get update \
#    && apt-get install -y software-properties-common \
#    && apt-add-repository -y ppa:nginx/stable \
#    && apt-get update \
#    && apt-get install -y nginx \
#    && rm -rf /var/lib/apt/lists/*

#ADD conf/nginx.conf /etc/nginx/nginx.conf
#ADD conf/jenkins.conf /etc/nginx/conf.d/jenkins.conf

#ADD data/www /data/www

#RUN rm /etc/nginx/sites-enabled/default

#RUN ln -sf /dev/stdout /var/log/nginx/access.log \
#   && ln -sf /dev/stderr /var/log/nginx/error.log

#EXPOSE 80
#EXPOSE 443

#CMD ["nginx", "-g", "daemon off;"]
