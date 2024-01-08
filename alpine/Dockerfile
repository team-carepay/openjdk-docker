ARG ALPINE_VERSION=3.18
FROM python:3.10-alpine${ALPINE_VERSION} as builder

ARG JDK_VERSION=11
ARG AWSCLI_VERSION=2.15.5


RUN apk add --no-cache git unzip groff build-base libffi-dev cmake
RUN git clone --single-branch --depth 1 -b ${AWSCLI_VERSION} https://github.com/aws/aws-cli.git

WORKDIR aws-cli
RUN ./configure --with-install-type=portable-exe --with-download-deps
RUN make
RUN make install

# reduce image size: remove autocomplete and examples
RUN rm -rf \
    /usr/local/lib/aws-cli/aws_completer \
    /usr/local/lib/aws-cli/awscli/data/ac.index \
    /usr/local/lib/aws-cli/awscli/examples
RUN find /usr/local/lib/aws-cli/awscli/data -name completions-1*.json -delete
RUN find /usr/local/lib/aws-cli/awscli/botocore/data -name examples-1.json -delete
RUN (cd /usr/local/lib/aws-cli; for a in *.so*; do test -f /lib/$a && rm $a; done)

# build the final image
FROM alpine:${ALPINE_VERSION}
ARG JDK_VERSION=11
COPY --from=builder /usr/local/lib/aws-cli/ /usr/local/lib/aws-cli/
RUN ln -s /usr/local/lib/aws-cli/aws /usr/local/bin/aws

# install glibc compatibility for alpine
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
        openjdk${JDK_VERSION} \
        openssh-client \
        yq \
    && \
    rm -rf /var/cache/apk/* /usr/lib/jvm/default-jvm/demo /usr/lib/jvm/default-jvm/man

ENTRYPOINT ["/bin/bash"]
