ARG ALPINE_VERSION=3.18
FROM python:3.10-alpine${ALPINE_VERSION} as builder

ARG JDK_VERSION
ARG AWS_CLI_VERSION

RUN \
    if [ -n "${AWS_CLI_VERSION}" ]; then \
        apk add --no-cache \
             build-base \
             cmake \
             git \
             groff \
             libffi-dev \
             unzip \
        && \
        git clone --single-branch --depth 1 -b ${AWS_CLI_VERSION} https://github.com/aws/aws-cli.git && \
        (cd aws-cli && ./configure --with-install-type=portable-exe --with-download-deps && make && make install) && \
        rm -rf \
            /usr/local/lib/aws-cli/aws_completer \
            /usr/local/lib/aws-cli/awscli/data/ac.index \
            /usr/local/lib/aws-cli/awscli/examples \
        && \
        find /usr/local/lib/aws-cli/awscli/data -name completions-1*.json -delete && \
        find /usr/local/lib/aws-cli/awscli/botocore/data -name examples-1.json -delete && \
        (cd /usr/local/lib/aws-cli && for a in *.so*; do test -f /lib/$a && rm $a; done) \
        ; \
    else \
        mkdir -p /usr/local/lib/aws-cli/ && \
        touch /usr/local/lib/aws-cli/aws \
        ; \
    fi

# build the final image
FROM alpine:${ALPINE_VERSION}

ARG JDK_VERSION

COPY --from=builder /usr/local/lib/aws-cli/ /usr/local/lib/aws-cli/
RUN ln -s /usr/local/lib/aws-cli/aws /usr/local/bin/aws

RUN \
    echo "https://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories && \
    echo "https://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories && \
    apk add --no-cache \
        bash \
        curl \
        docker-cli \
        docker-compose \
        git \
        jq \
        kubectl \
        openssh-client \
        yq \
    && \
    if [ "${JDK_VERSION}" != "nojdk" ]; then \
        apk add --no-cache \
            openjdk${JDK_VERSION} \
        && \
        rm -rf /usr/lib/jvm/default-jvm/demo /usr/lib/jvm/default-jvm/man \
        ; \
    fi && \
    rm -rf /var/cache/apk/*

ENTRYPOINT ["/bin/bash"]
