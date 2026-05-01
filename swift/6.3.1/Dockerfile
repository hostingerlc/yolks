FROM        --platform=$TARGETOS/$TARGETARCH swift:6.3.1-bookworm-slim
LABEL       author="Lezetho" maintainer="im@lezetho.xyz"

RUN         useradd -m -d /home/container container
STOPSIGNAL  SIGINT

RUN         apt update \
            && apt -y install \
                ca-certificates \
                curl \
                tar \
                zip \
                git \
                iproute2 \
                iputils-ping \
                dnsutils \
                tzdata \
                tini \
            && apt clean \
            && rm -rf /var/lib/apt/lists/*

RUN         rm -rf /usr/bin/dd \
            && rm -rf /usr/bin/fallocate \
            && rm -rf /usr/bin/truncate \
            && rm -rf /usr/bin/xfs_mkfile

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

COPY        --chown=container:container ./entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh

ENTRYPOINT  ["/usr/bin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]
