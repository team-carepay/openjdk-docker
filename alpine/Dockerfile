ARG ALPINE_VERSION=3.23.3
FROM alpine:${ALPINE_VERSION}

ARG JDK_VERSION
ARG AWS_CLI_VERSION

RUN \
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
    curl -fsSL https://github.com/daveshanley/vacuum/releases/download/v0.17.11/vacuum_0.17.11_linux_x86_64.tar.gz | tar xz -C /usr/local/bin vacuum && \
    curl -fsSL https://github.com/google/yamlfmt/releases/download/v0.17.2/yamlfmt_0.17.2_Linux_x86_64.tar.gz | tar xz -C /usr/local/bin yamlfmt && \
    if [ -n "${AWS_CLI_VERSION}" ]; then \
        apk add --no-cache \
            aws-cli \
        && mkdir -p ~/.docker \
        && echo '{ "credsStore": "ecr-login" }' > ~/.docker/config.json \
        ; \
    fi && \
    rm -rf /var/cache/apk/*

ENTRYPOINT ["/bin/bash"]
