ARG VERSION=11

FROM azul/zulu-openjdk-alpine:$VERSION

ARG AWS
ARG YQ=v4.6.0
ARG GLIBC_VER=2.32-r0

# install glibc compatibility for alpine
RUN \
    apk add --no-cache \
        curl \
        git \
        jq \
        openssh-client \
    && \
    curl -sL https://github.com/mikefarah/yq/releases/download/$YQ/yq_linux_amd64 -o /usr/bin/yq && \
    chmod +x /usr/bin/yq && \
    rm -rf /var/cache/apk/*

RUN \
    if [ -n "$AWS" ]; then \
      apk add --no-cache --virtual binutils-pack \
          binutils \
      && \
      curl -sL https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub -o /etc/apk/keys/sgerrand.rsa.pub && \
      curl -sLO https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VER}/glibc-${GLIBC_VER}.apk && \
      curl -sLO https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VER}/glibc-bin-${GLIBC_VER}.apk && \
      apk add --no-cache --virtual binutils \
          glibc-${GLIBC_VER}.apk \
          glibc-bin-${GLIBC_VER}.apk \
      && \
      apk --no-cache del \
          binutils-pack \
      && \
      rm glibc-${GLIBC_VER}.apk && \
      rm glibc-bin-${GLIBC_VER}.apk && \
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
      (cd /usr/local/aws-cli/v2/current/dist/botocore/data && find . -name 'examples*.json' | xargs rm -f) \
      ; \
    fi

ENV JAVA_HOME=/usr/lib/jvm/default-jvm
ENV PATH=$PATH:$JAVA_HOME/bin
