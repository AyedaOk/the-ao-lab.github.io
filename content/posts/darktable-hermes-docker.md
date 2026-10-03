---
title: "Run Darktable MCP with Hermes in Docker"
date: 2026-09-28
description: "Build Darktable master and Hermes in an isolated browser-based Docker environment."
tags: ["darktable", "mcp", "hermes", "docker", "linux"]
showShare: false
---

This setup runs **Darktable master (or specific PR)** in one Docker container. The desktop is provided by [LinuxServer Webtop](https://docs.linuxserver.io/images/docker-webtop/) and is accessed from a browser.

## Files

Create a folder containing `Dockerfile`, `compose.yaml`, and `.dockerignore`.

### Dockerfile

```dockerfile
FROM lscr.io/linuxserver/webtop:debian-kde

ENV DEBIAN_FRONTEND=noninteractive

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# ------------------------------------------------------------
# Darktable build dependencies
# ------------------------------------------------------------
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    git \
    build-essential \
    cmake \
    ninja-build \
    pkg-config \
    gettext \
    intltool \
    xsltproc \
    po4a \
    python3-jsonschema \
    desktop-file-utils \
    libarchive-dev \
    libatk1.0-dev \
    libcairo2-dev \
    libcolord-dev \
    libcurl4-gnutls-dev \
    libexiv2-dev \
    libgdk-pixbuf-2.0-dev \
    libglib2.0-dev \
    libgtk-3-dev \
    libinih-dev \
    libjson-glib-dev \
    liblcms2-dev \
    liblensfun-dev \
    libjpeg-dev \
    libpng-dev \
    libpotrace-dev \
    libpugixml-dev \
    librsvg2-dev \
    libsecret-1-dev \
    libsqlite3-dev \
    libtiff-dev \
    libwebp-dev \
    libxml2-dev \
    zlib1g-dev \
    libgphoto2-dev \
    libopenexr-dev \
    libheif-dev \
    libavif-dev \
    libjxl-dev \
    liblua5.4-dev \
    libgmic-dev \
    libgraphicsmagick1-dev \
    && rm -rf /var/lib/apt/lists/*

# ------------------------------------------------------------
# Darktable development branch / PR
# ------------------------------------------------------------
ARG DARKTABLE_REF=master

RUN git clone https://github.com/darktable-org/darktable.git /tmp/darktable \
    && cd /tmp/darktable \
    && if echo "${DARKTABLE_REF}" | grep -q '^pr-'; then \
         PR_NUM="${DARKTABLE_REF#pr-}"; \
         git fetch origin "pull/${PR_NUM}/head:${DARKTABLE_REF}"; \
       fi \
    && git checkout "${DARKTABLE_REF}" \
    && git submodule update --init \
    && cmake -S . -B build -G Ninja \
         -DCMAKE_BUILD_TYPE=Release \
         -DCMAKE_INSTALL_PREFIX=/opt/darktable \
         -DUSE_MCP=ON \
    && cmake --build build --parallel "$(nproc)" \
    && cmake --install build \
    && ldconfig \
    && rm -rf /tmp/darktable

RUN ln -s /opt/darktable/bin/darktable /usr/local/bin/darktable \
    && ln -s /opt/darktable/bin/darktable-mcp /usr/local/bin/darktable-mcp \
    && ln -s /opt/darktable/share/applications/org.darktable.darktable.desktop \
             /usr/share/applications/org.darktable.darktable.desktop

```

### compose.yaml

```yaml
services:
  darktable:
    build:
      context: .
      args:
        DARKTABLE_REF: master # Or pr-XXXXX

    container_name: darktable-hermes

    environment:
      PUID: 1000
      PGID: 1000
      TZ: America/Toronto
      HERMES_HOME: /config/hermes

    volumes:
      - ./config:/config

      # Test photo directory
      - /home/ao/Pictures/Local/YouTube/Darktable 5.7.0/:/share

    ports:
      - "127.0.0.1:3001:3001"

    shm_size: "1gb"

    restart: unless-stopped
```

Change `/home/ao/Pictures` to the folder you want the container to access. You can also do `/share:ro` to keeps the originals read-only. 

### .dockerignore

```text
config/
```

This prevents Webtop runtime sockets inside `config/` from being included in the Docker build context.

## Build and run

```sh
docker compose build --no-cache
docker compose up -d
```

Open:

```text
https://127.0.0.1:3001
```

## Install OpenCode or Hermes

For Opencode:

`curl -fsSL https://opencode.ai/v2/install | bash`

Add the Darktable MCP server:

```sh
opencode mcp add darktable --global -- \
  /opt/darktable/bin/darktable-mcp \
  --core \
  --configdir /config/.config/darktable
```

For Hermes:

```sh
curl -fsSL https://hermes-agent.nousresearch.com/install.sh \
  | bash -s -- --dir /config/hermes/hermes-agent \
      --hermes-home /config/hermes \
      --skip-browser --skip-computer-use
```

Then setup Hermes:

```sh
hermes setup
hermes mcp add darktable \
  --command /opt/darktable/bin/darktable-mcp \
  --args --core --configdir /config/.config/darktable
```
Then start Hermes:

```sh
hermes
```

Close Darktable before starting an agent connected to its catalog. Run only one MCP client against that catalog at a time. Close the agent before reopening Darktable.

## Update

```sh
docker compose build --pull --no-cache
```

Darktable MCP documentation: [darktable/src/mcp/README.md](https://github.com/darktable-org/darktable/blob/master/src/mcp/README.md)
