FROM        --platform=$TARGETOS/$TARGETARCH node:24-bookworm-slim

LABEL       author="Lezetho" maintainer="im@lezetho.xyz"

RUN         useradd -m -d /home/container container
STOPSIGNAL SIGINT

RUN         apt update \
            && apt -y install \
                ffmpeg \
                iproute2 \
                git \
                sqlite3 \
                libsqlite3-dev \
                python3 \
                python3-dev \
                python3-pip \
                python3-venv \
                python3-full \
                ca-certificates \
                dnsutils \
                tzdata \
                zip \
                tar \
                curl \
                build-essential \
                libtool \
                iputils-ping \
                libnss3 \
                tini \
            && apt clean \
            && rm -rf /var/lib/apt/lists/*

RUN         rm /usr/lib/python3.*/EXTERNALLY-MANAGED || true

RUN rm -rf /usr/bin/dd \
 && rm -rf /usr/bin/fallocate \
 && rm -rf /usr/bin/truncate \
 && rm -rf /usr/bin/xfs_mkfile 

RUN         npm install --global npm@latest typescript ts-node @types/node

# install pnpm
RUN         npm install -g corepack
RUN         corepack enable
RUN         corepack prepare pnpm@latest --activate

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

COPY        --chown=container:container ./entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh

ENTRYPOINT    ["/usr/bin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]
