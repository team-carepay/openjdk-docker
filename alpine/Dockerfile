ARG ALPINE_VERSION=3.22.1
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
    if [ -n "${AWS_CLI_VERSION}" ]; then \
        apk add --no-cache \
            aws-cli \
        && mkdir -p ~/.docker \
        && echo '{ "credsStore": "ecr-login" }' > ~/.docker/config.json \
        ; \
    fi && \
    rm -rf /var/cache/apk/*

ENTRYPOINT ["/bin/bash"]
