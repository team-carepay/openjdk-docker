ARG VERSION=8-jdk

FROM openjdk:$VERSION-slim-buster

ARG AWS
ARG YQ=v4.6.0
ENV DEBIAN_FRONTEND=noninteractive
RUN \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        apt-transport-https \
        ca-certificates \
        curl \
        git \
        gnupg2 \
        jq \
        lsb-release \
        software-properties-common \
        openssh-client \
        unzip \
    && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        docker-ce-cli \
    && \
    curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
    chmod a+x /usr/local/bin/docker-compose && \
    curl -sL https://github.com/mikefarah/yq/releases/download/$YQ/yq_linux_amd64 -o /usr/bin/yq && \
    chmod +x /usr/bin/yq && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean

RUN \
    if [ -n "$AWS" ]; then \
      curl -sL https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o awscliv2.zip && \
      unzip -q awscliv2.zip && \
      aws/install && \
      rm -rf \
          awscliv2.zip \
          aws \
          /usr/local/aws-cli/v2/current/dist/aws_completer \
          /usr/local/aws-cli/v2/current/dist/awscli/data/ac.index \
          /usr/local/aws-cli/v2/current/dist/awscli/examples \
      && \
      find /usr/local/aws-cli/v2/current/dist/awscli/botocore/data -name examples-1.json -delete; \
    fi
