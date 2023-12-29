ARG VERSION=11

FROM azul/zulu-openjdk-alpine:$VERSION

ARG AWS
ARG YQ=v4.30.8
ARG GLIBC_VER=2.35-r0
ARG AWSCLI_VERSION=2.15.5

# install glibc compatibility for alpine
RUN \
    apk add --no-cache \
        bash \
        curl \
        docker-cli \
        git \
        jq \
        openssh-client \
    && \
    curl -L https://raw.githubusercontent.com/docker/compose-cli/main/scripts/install/install_linux.sh | sh && \
    curl -sL https://github.com/mikefarah/yq/releases/download/3.4.1/yq_linux_amd64 -o /usr/local/bin/yq3 && \
    chmod +x /usr/local/bin/yq3 && \
    curl -sL https://github.com/mikefarah/yq/releases/download/${YQ}/yq_linux_amd64 -o /usr/local/bin/yq && \
    chmod +x /usr/local/bin/yq && \
    rm -rf /var/cache/apk/*

RUN \
    if [ -n "$AWS" ]; then \
      apk add --no-cache --virtual binutils-pack \
          binutils \
      && \
      curl -sL https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub -o /etc/apk/keys/sgerrand.rsa.pub && \
      curl -sLO https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VER}/glibc-${GLIBC_VER}.apk && \
      curl -sLO https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VER}/glibc-bin-${GLIBC_VER}.apk && \
      apk add --force-overwrite glibc-${GLIBC_VER}.apk && \
      apk add --force-overwrite glibc-bin-${GLIBC_VER}.apk && \
      ln -sf /usr/glibc-compat/lib/ld-linux-x86-64.so.2 /lib64/ld-linux-x86-64.so.2 && \
      curl -sL https://awscli.amazonaws.com/awscli-exe-linux-x86_64-${AWSCLI_VERSION}.zip -o awscliv2.zip && \
      unzip -q awscliv2.zip && \
      aws/install && \
      apk add --no-cache \
         docker-credential-ecr-login \
      && \
      rm -rf \
          /var/cache/apk/* \
          awscliv2.zip \
          aws \
          /usr/local/aws-cli/v2/current/dist/aws_completer \
          /usr/local/aws-cli/v2/current/dist/awscli/data/ac.index \
          /usr/local/aws-cli/v2/current/dist/awscli/examples \
      && \
      find /usr/local/aws-cli/v2/current/dist/awscli/botocore/data -name examples-1.json -delete; \
    fi

ENV JAVA_HOME=/usr/lib/jvm/default-jvm
ENV PATH=$PATH:$JAVA_HOME/bin
ENTRYPOINT ["/bin/bash"]
