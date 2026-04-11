ARG ALPINE_VERSION=3.23.3
FROM alpine:${ALPINE_VERSION}

ARG JDK_VERSION=21
ARG AWS_CLI_VERSION

RUN \
    if [ -n "${AWS_CLI_VERSION}" ]; then \
        apk add --no-cache \
            aws-cli \
        && mkdir -p ~/.docker \
        && echo '{ "credsStore": "ecr-login" }' > ~/.docker/config.json \
        ; \
    fi && \
    echo "https://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories && \
    echo "https://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories && \
    apk add --no-cache \
        bash \
        curl \
        docker-cli \
        docker-cli-compose \
        docker-credential-ecr-login \
        git \
        jq \
        kubectl \
        openssh-client \
        yq \
    && \
    if [ "${JDK_VERSION}" != "nojdk" ]; then \
        wget -O /etc/apk/keys/amazoncorretto.rsa.pub https://apk.corretto.aws/amazoncorretto.rsa.pub && \
        echo "https://apk.corretto.aws" >> /etc/apk/repositories && \
        apk add --no-cache \
            amazon-corretto-${JDK_VERSION} \
        && \
        rm -rf /usr/lib/jvm/default-jvm/demo /usr/lib/jvm/default-jvm/man \
        ; \
    fi && \
    curl -fsSL https://github.com/daveshanley/vacuum/releases/download/v0.25.8/vacuum_0.25.8_linux_x86_64.tar.gz | tar xz -C /usr/local/bin vacuum && \
    curl -fsSL https://github.com/google/yamlfmt/releases/download/v0.21.0/yamlfmt_0.21.0_Linux_x86_64.tar.gz | tar xz -C /usr/local/bin yamlfmt && \
    rm -rf /var/cache/apk/*

ENTRYPOINT ["/bin/bash"]
