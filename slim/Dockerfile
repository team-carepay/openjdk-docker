ARG VERSION=8-jdk

FROM openjdk:$VERSION-slim-buster

ARG AWS

RUN \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        curl \
        git \
        jq \
        openssh-client \
        unzip \
    && \
    curl https://github.com/mikefarah/yq/releases/download/v4.2.0/yq_linux_amd64 -o /usr/bin/yq && \
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
          /usr/local/aws-cli/v2/*/dist/aws_completer \
          /usr/local/aws-cli/v2/*/dist/awscli/data/ac.index \
          /usr/local/aws-cli/v2/*/dist/awscli/examples \
      && \
      (cd /usr/local/aws-cli/v2/current/dist/botocore/data && find . -name 'examples*.json' | xargs rm -f); \
    fi
