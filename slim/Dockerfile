FROM debian:trixie-slim
ARG JDK_VERSION
ARG AWS_CLI_VERSION

ENV DEBIAN_FRONTEND=noninteractive
RUN \
    apt-get -qq update && \
    apt-get -qq install -y --no-install-recommends \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg2 \
    && \
    curl -fsSL https://apt.corretto.aws/corretto.key | gpg --dearmor -o /etc/apt/trusted.gpg.d/corretto.gpg && \
    echo "deb [arch=amd64] https://apt.corretto.aws stable main" > /etc/apt/sources.list.d/corretto.list && \
    echo "deb [arch=amd64] http://deb.debian.org/debian testing main" > /etc/apt/sources.list.d/testing.list && \
    curl -fsSL https://dl.k8s.io/apt/doc/apt-key.gpg | gpg --dearmor -o /etc/apt/trusted.gpg.d/k8s.gpg && \
    curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | gpg --dearmor -o /etc/apt/trusted.gpg.d/k8s-release.gpg && \
    echo "deb [arch=amd64] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /" > /etc/apt/sources.list.d/k8s.list && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg && \
    echo "deb [arch=amd64] https://download.docker.com/linux/debian bookworm stable" > /etc/apt/sources.list.d/docker.list && \
    apt-get -qq update && \
    apt-get -qq install -y --no-install-recommends \
        bash \
        docker-ce-cli \
        docker-compose-plugin \
        git \
        jq \
        kubectl \
        openssh-client \
        unzip \
        yq \
    && \
    if [ "${JDK_VERSION}" != "nojdk" ]; then \
        apt-get -qq install -y --no-install-recommends \
            java-${JDK_VERSION}-amazon-corretto-jdk \
        ; \
    fi && \
    if [ -n "${AWS_CLI_VERSION}" ]; then \
        apt-get -qq install -y --no-install-recommends \
            awscli \
            amazon-ecr-credential-helper \
        && mkdir -p ~/.docker \
        && echo '{ "credsStore": "ecr-login" }' > ~/.docker/config.json \
        ; \
    fi && \
    curl -sL https://download.docker.com/linux/static/stable/x86_64/docker-26.1.4.tgz | tar xzv --strip 1 -C /usr/local/bin docker/docker && \
    curl -fsSL https://github.com/daveshanley/vacuum/releases/download/v0.17.11/vacuum_0.17.11_linux_x86_64.tar.gz | tar xz -C /usr/local/bin vacuum && \
    curl -fsSL https://github.com/google/yamlfmt/releases/download/v0.17.2/yamlfmt_0.17.2_Linux_x86_64.tar.gz | tar xzv -C /usr/local/bin yamlfmt && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get -qq clean


ENTRYPOINT ["/bin/bash"]
