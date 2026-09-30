FROM jenkins/jenkins:lts-jdk17

# Official Jenkins image doesn't include the Docker CLI, since Jenkins
# itself is container agnostic. We add just the CLI client here, not
# a full Docker daemon, since this container talks to the DinD sidecar
# over TLS rather than running its own daemon.
USER root
RUN apt-get update && \
    apt-get install -y ca-certificates curl gnupg && \
    install -m 0755 -d /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    chmod a+r /etc/apt/keyrings/docker.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian $(. /etc/os-release && echo $VERSION_CODENAME) stable" > /etc/apt/sources.list.d/docker.list && \
    apt-get update && \
    apt-get install -y docker-ce-cli && \
    rm -rf /var/lib/apt/lists/*

# Drop back to the non root jenkins user for everything else,
# root was only needed to install the package above.
USER jenkins
