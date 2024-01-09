FROM debian:bookworm-slim
ARG JDK_VERSION
ARG AWS_CLI_VERSION

ENV DEBIAN_FRONTEND=noninteractive
RUN \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg2 \
    && \
    echo "deb [arch=amd64] http://deb.debian.org/debian testing main" > /etc/apt/sources.list.d/testing.list && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg && \
    echo "deb [arch=amd64] https://download.docker.com/linux/debian bookworm stable" > /etc/apt/sources.list.d/docker.list && \
    curl -fsSL https://dl.k8s.io/apt/doc/apt-key.gpg | gpg --dearmor -o /etc/apt/trusted.gpg.d/k8s.gpg && \
    curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | gpg --dearmor -o /etc/apt/trusted.gpg.d/k8s-release.gpg && \
    echo "deb [arch=amd64] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /" > /etc/apt/sources.list.d/k8s.list && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        bash \
        docker-ce-cli \
        git \
        jq \
        kubectl \
        openssh-client \
        unzip \
        yq \
    && \
    if [ "${JDK_VERSION}" != "nojdk" ]; then \
        apt-get install -y --no-install-recommends \
            openjdk-${JDK_VERSION}-jdk \
        ; \
    fi && \
    if [ -n "${AWS_CLI_VERSION}" ]; then \
        apt-get install -y --no-install-recommends \
            awscli \
        ; \
    fi && \
    curl -L "https://github.com/docker/compose/releases/download/v2.23.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
    chmod a+x /usr/local/bin/docker-compose && \
    curl -sL https://github.com/mikefarah/yq/releases/download/3.4.1/yq_linux_amd64 -o /usr/local/bin/yq3 && \
    chmod +x /usr/local/bin/yq3 && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean

ENTRYPOINT ["/bin/bash"]
