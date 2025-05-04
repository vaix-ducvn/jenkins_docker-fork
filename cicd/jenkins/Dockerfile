# ci-cd/docker/Dockerfile.jenkins
FROM jenkins/jenkins:lts

# Tắt wizard cài đặt
ENV JAVA_OPTS=-Djenkins.install.runSetupWizard=false
# Copy và cài plugin
COPY /cicd/jenkins/plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN jenkins-plugin-cli --plugin-file /usr/share/jenkins/ref/plugins.txt

# Copy và cấu hình JCasC
COPY /cicd/jenkins/jenkins.yaml /usr/share/jenkins/ref/jenkins.yaml
ENV CASC_JENKINS_CONFIG=/usr/share/jenkins/ref/jenkins.yaml

# Cài Node.js và thiết lập timezone
USER root
RUN apt-get update && apt-get install -y curl tzdata sudo && \
    curl -fsSL https://deb.nodesource.com/setup_22.x | bash - && \
    apt-get install -y nodejs && \
    rm -rf /var/lib/apt/lists/* && \
    ln -sf /usr/share/zoneinfo/Asia/Ho_Chi_Minh /etc/localtime && \
    echo "Asia/Ho_Chi_Minh" > /etc/timezone && \
    dpkg-reconfigure -f noninteractive tzdata

# Tạo thư mục dist
RUN mkdir -p /var/jenkins_home/dist && chown jenkins:jenkins /var/jenkins_home/dist

# Expose port
EXPOSE 8080 50000