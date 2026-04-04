FROM node:24-bookworm-slim AS node

FROM mcr.microsoft.com/powershell:7.5-debian-bookworm-slim AS powershell

FROM mcr.microsoft.com/dotnet/sdk:10.0-bookworm-slim

ENV DOTNET_NOLOGO=1 \
    DOTNET_SKIP_FIRST_TIME_EXPERIENCE=1 \
    DOTNET_CLI_TELEMETRY_OPTOUT=1 \
    DOTNET_CLI_HOME=/workspaces/workiq-mcp/.dotnet-home \
    NUGET_PACKAGES=/workspaces/workiq-mcp/.nuget/packages \
    NUGET_XMLDOC_MODE=skip \
    NPM_CONFIG_UPDATE_NOTIFIER=false \
    NPM_CONFIG_FUND=false \
    npm_config_cache=/workspaces/workiq-mcp/.npm-cache

COPY --from=node /usr/local/ /usr/local/
COPY --from=powershell /opt/microsoft/powershell /opt/microsoft/powershell

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        git \
        less \
        procps \
        tar \
    && ln -s /opt/microsoft/powershell/7/pwsh /usr/bin/pwsh \
    && mkdir -p "$DOTNET_CLI_HOME" "$NUGET_PACKAGES" "$npm_config_cache" \
    && node --version \
    && npm --version \
    && pwsh --version \
    && dotnet --version \
    && rm -rf /var/lib/apt/lists/*

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
    && rm -rf /var/lib/apt/lists/*

# Generate the desired locale (en_US.UTF-8)
RUN locale-gen en_US.UTF-8

# Make typing unicode characters in the terminal work.
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8

# Remove the `ubuntu` user and add a user `coder` so that you're not developing as the `root` user
RUN useradd coder \
    --create-home \
    --shell=/bin/bash \
    --uid=1000 \
    --user-group && \
    echo "coder ALL=(ALL) NOPASSWD:ALL" >>/etc/sudoers.d/nopasswd

USER coder
