FROM jenkins/jenkins:lts-jdk17
USER root
RUN apt-get update
RUN apt-get install -y maven git unzip curl wget gnupg sudo
# RUN apt-get install -y docker.io # runing docker in docker container is complicated

RUN curl -LO https://dl.k8s.io/release/v1.32.0/bin/linux/amd64/kubectl
RUN install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

RUN wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | tee /usr/share/keyrings/trivy.gpg > /dev/null
RUN echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb generic main" | tee -a /etc/apt/sources.list.d/trivy.list
RUN apt-get update
RUN apt-get install trivy

RUN wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.6.2.2472-linux.zip
RUN unzip sonar-scanner-cli-4.6.2.2472-linux.zip
RUN mv sonar-scanner-4.6.2.2472-linux /opt/sonar-scanner
RUN rm sonar-scanner-cli-4.6.2.2472-linux.zip

RUN usermod -aG sudo jenkins
RUN echo "jenkins ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# USER jenkins # switch back to jenkins user is best practice