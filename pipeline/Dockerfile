FROM jenkins/jenkins

USER root

# Install docker
RUN apt-get update && \
    apt-get -y install apt-transport-https ca-certificates curl gnupg2 software-properties-common && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" && \
    apt-get update && \
    apt-get -y install docker-ce

RUN apt-get install -y python3 python3-pip

# Install python ansible
RUN pip install -U ansible

# Install compose
RUN curl -L https://github.com/docker/compose/releases/download/1.25.3/docker-compose-`uname -s`-`uname -m` \
    -o /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose

# Add jenkins user to run docker commands
RUN usermod -aG docker jenkins

USER jenkins


