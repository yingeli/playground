FROM node:24

USER root
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    bash \
    ca-certificates \
    curl \
    git \
    git-lfs \
    jq \
    locales \
    nano \
    sudo \
    libdbus-1-3 \
    libwebkit2gtk-4.0-37 \
    && rm -rf /var/lib/apt/lists/*

USER node
