ARG ALPINEVERSION=3.11

FROM alpine:$ALPINEVERSION

ARG VERSION="11"
ARG AWS
ARG GLIBC_VER=2.31-r0

# install glibc compatibility for alpine
RUN \
    apk add --no-cache \
        curl \
        openjdk$VERSION \
        git \
        jq \
    && \
    rm -rf /var/cache/apk/*

RUN if [ -n "$AWS" ]; then \
    apk add --no-cache \
        binutils \
    && \
    curl -sL https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub -o /etc/apk/keys/sgerrand.rsa.pub && \
    curl -sLO https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VER}/glibc-${GLIBC_VER}.apk && \
    curl -sLO https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VER}/glibc-bin-${GLIBC_VER}.apk && \
    apk add --no-cache \
        glibc-${GLIBC_VER}.apk \
        glibc-bin-${GLIBC_VER}.apk \
    && \
    apk --no-cache del \
        binutils \
    && \
    rm glibc-${GLIBC_VER}.apk && \
    rm glibc-bin-${GLIBC_VER}.apk && \
    curl -sL https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o awscliv2.zip && \
    unzip awscliv2.zip && \
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
