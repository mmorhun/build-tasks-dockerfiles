ARG version=37.74.1
FROM registry.access.redhat.com/ubi9/ubi-minimal@sha256:582e18f13291d7c686ec4e6e92d20b24c62ae0fc72767c46f30a69b1a6198055

LABEL name="renovate"
LABEL org.opencontainers.image.source="https://github.com/renovatebot/renovate" \
  org.opencontainers.image.url="https://renovatebot.com"

RUN microdnf -y install npm docker shadow-utils git python make g++ wget xz && \
    groupadd -r renovate -g 101 && \
    useradd -u 101 -r -g root -G renovate -m -d /home/renovate -s /sbin/nologin -c "Renovate user" renovate && \
    chmod 0770 /home/renovate

RUN wget https://nodejs.org/dist/v20.10.0/node-v20.10.0-linux-x64.tar.xz && \
    echo '3fe4ec5d70c8b4ffc1461dec83ab23fc70124e137c4cbbe1ccc9d6ae6ec04a7d  node-v20.10.0-linux-x64.tar.xz' | sha256sum -c - && \
    mkdir -p /opt/node && \
    tar -C /opt -xvf node-v20.10.0-linux-x64.tar.xz

ENV PATH="/opt/node-v20.10.0-linux-x64/bin:$PATH:/home/renovate/node_modules/.bin"
WORKDIR /home/renovate
USER 101
RUN npm install renovate@$RENOVATE_VERSION re2

ENV RENOVATE_X_IGNORE_NODE_WARN=true

ENTRYPOINT ["renovate"]

RUN set -ex; \
  renovate --version; \
  renovate-config-validator; \
  node -e "new require('re2')('.*').exec('test')"; \
  true

LABEL org.opencontainers.image.version="${RENOVATE_VERSION}"
