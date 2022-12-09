ARG ALPINEVERSION=3.15

FROM alpine:$ALPINEVERSION

ARG VERSION=11
ARG AWS
ARG YQ=v4.6.0
ARG GLIBC_VER=2.35-r0
ARG AWSCLI_VERSION=2.6.1

# install glibc compatibility for alpine
RUN \
    echo "https://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories && \
    apk add --no-cache \
        bash \
        curl \
        docker-cli \
        docker-compose \
        git \
        jq \
        kubectl \
        openjdk$VERSION \
        openssh-client \
    && \
    curl -sL https://github.com/mikefarah/yq/releases/download/$YQ/yq_linux_amd64 -o /usr/bin/yq && \
    chmod +x /usr/bin/yq && \
    rm -rf /var/cache/apk/* /usr/lib/jvm/default-jvm/demo /usr/lib/jvm/default-jvm/man

RUN \
    if [ -n "$AWS" ]; then \
      apk add --no-cache --virtual binutils-pack \
          binutils \
      && \
      curl -sL https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub -o /etc/apk/keys/sgerrand.rsa.pub && \
      curl -sLO https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VER}/glibc-${GLIBC_VER}.apk && \
      curl -sLO https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VER}/glibc-bin-${GLIBC_VER}.apk && \
      apk add --no-cache --virtual glibc-pack \
          glibc-${GLIBC_VER}.apk \
          glibc-bin-${GLIBC_VER}.apk \
      && \
      apk --no-cache del \
          binutils-pack \
      && \
      rm glibc-${GLIBC_VER}.apk && \
      rm glibc-bin-${GLIBC_VER}.apk && \
      ln -sf /usr/glibc-compat/lib/ld-linux-x86-64.so.2 /lib64/ld-linux-x86-64.so.2 && \
      curl -sL https://awscli.amazonaws.com/awscli-exe-linux-x86_64-${AWSCLI_VERSION}.zip -o awscliv2.zip && \
      unzip -q awscliv2.zip && \
      aws/install && \
      apk add --no-cache \
         docker-credential-ecr-login \
      && \
      rm -rf \
          awscliv2.zip \
          aws \
          /usr/local/aws-cli/v2/current/dist/aws_completer \
          /usr/local/aws-cli/v2/current/dist/awscli/data/ac.index \
          /usr/local/aws-cli/v2/current/dist/awscli/examples \
          /var/cache/apk/* \
      && \
      find /usr/local/aws-cli/v2/current/dist/awscli/botocore/data -name examples-1.json -delete; \
    fi
ENTRYPOINT ["/bin/bash"]
