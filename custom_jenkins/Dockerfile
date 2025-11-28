# Use the Jenkins image as the base image
FROM jenkins/jenkins:lts

# Switch to root user to install dependencies
USER root

# Install prerequisites (software-properties-common removed)
RUN apt-get update -y && \
    apt-get install -y ca-certificates curl gnupg lsb-release && \
    apt-get clean

# Add Docker official GPG key (new safe method)
RUN mkdir -p /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/debian/gpg \
    | gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    chmod a+r /etc/apt/keyrings/docker.gpg

# Add Docker repository (auto detects bookworm/trixie)
RUN echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/debian $(lsb_release -cs) stable" \
  > /etc/apt/sources.list.d/docker.list

# Install Docker inside Jenkins
RUN apt-get update -y && \
    apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin && \
    apt-get clean

# Add Jenkins user to the Docker group
RUN groupadd -f docker && usermod -aG docker jenkins

# Prepare Docker volume for DinD
RUN mkdir -p /var/lib/docker
VOLUME /var/lib/docker

# Return to default Jenkins user
USER jenkins
