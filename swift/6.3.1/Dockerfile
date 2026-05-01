FROM        --platform=$TARGETOS/$TARGETARCH debian:bookworm-slim
LABEL       author="Lezetho" maintainer="im@lezetho.xyz"

RUN         useradd -m -d /home/container container
STOPSIGNAL  SIGINT

ARG         SWIFT_VERSION=6.3.1
ARG         SWIFT_PLATFORM=debian12

RUN         apt update \
            && apt -y install \
                ca-certificates \
                curl \
                tar \
                zip \
                git \
                build-essential \
                libtool \
                pkg-config \
                binutils \
                libcurl4-openssl-dev \
                libxml2-dev \
                libssl-dev \
                libedit-dev \
                libsqlite3-dev \
                libz3-dev \
                libgcc-s1 \
                libstdc++6 \
                libatomic1 \
                libnss3 \
                iproute2 \
                iputils-ping \
                dnsutils \
                tzdata \
                tini \
            && apt clean \
            && rm -rf /var/lib/apt/lists/*

RUN         set -eux; \
            ARCH=$(dpkg --print-architecture); \
            if [ "$ARCH" = "amd64" ]; then SWIFT_ARCH="x86_64"; \
            elif [ "$ARCH" = "arm64" ]; then SWIFT_ARCH="aarch64"; \
            else echo "Unsupported arch: $ARCH" && exit 1; fi; \
            SWIFT_TAG="swift-${SWIFT_VERSION}-RELEASE"; \
            SWIFT_TARBALL="${SWIFT_TAG}-${SWIFT_PLATFORM}-${SWIFT_ARCH}.tar.gz"; \
            SWIFT_PLATFORM_DIR=$(echo "$SWIFT_PLATFORM" | tr -d '.'); \
            curl -fSL "https://download.swift.org/swift-${SWIFT_VERSION}-release/${SWIFT_PLATFORM_DIR}/${SWIFT_TAG}/${SWIFT_TARBALL}" \
                -o /tmp/swift.tar.gz; \
            tar -xzf /tmp/swift.tar.gz -C /usr/local --strip-components=1; \
            rm /tmp/swift.tar.gz; \
            swift --version

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
