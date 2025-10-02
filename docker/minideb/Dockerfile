# syntax=docker/dockerfile:1.4
################################################################################
# VCNNGR Minideb Base Image
#
# Minimal Debian-based image ottimizzata per container VCNNGR
# Ispirata a Bitnami Minideb ma brandizzata VCNNGR
#
# Copyright (c) 2025 VCNNGR
# SPDX-License-Identifier: MIT
#
# Build: docker build -t vcnngr/minideb:bookworm .
################################################################################

FROM debian:bookworm-slim

# Labels
LABEL org.opencontainers.image.authors="vcnngr" \
    org.opencontainers.image.description="Minimal Debian base image for VCNNGR containers" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.ref.name="bookworm" \
    org.opencontainers.image.source="https://github.com/vcnngr/jenkins" \
    org.opencontainers.image.title="minideb" \
    org.opencontainers.image.vendor="VCNNGR" \
    org.opencontainers.image.version="bookworm"

# Environment variables
ENV DEBIAN_FRONTEND=noninteractive \
    LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    HOME=/root

# Install essential packages
RUN set -eux; \
    apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    wget \
    gnupg \
    dirmngr \
    procps \
    gosu \
    bzip2 \
    gzip \
    xz-utils \
    unzip \
    less \
    vim-tiny \
    netcat-openbsd \
    openssl \
    locales \
    ; \
    echo "en_US.UTF-8 UTF-8" > /etc/locale.gen; \
    locale-gen en_US.UTF-8; \
    apt-get clean; \
    rm -rf \
    /var/lib/apt/lists/* \
    /tmp/* \
    /var/tmp/* \
    /usr/share/doc/* \
    /usr/share/man/* \
    /usr/share/info/* \
    /var/cache/debconf/*

# Copy install_packages script
COPY rootfs/usr/sbin/install_packages /usr/sbin/install_packages

# Make install_packages executable
RUN chmod +x /usr/sbin/install_packages

# Verify gosu works
RUN gosu nobody true

# Create vcnngr directories
RUN mkdir -p /opt/vcnngr /vcnngr

# Set working directory
WORKDIR /

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD [ "true" ]

# Default command
CMD ["/bin/bash"]