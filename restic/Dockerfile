ARG BUILD_FROM=hassioaddons/base:7.0.3
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]


# Setup base
RUN apk add --no-cache \
	heirloom-mailx \
	unzip \
    \
    && if [[ "${BUILD_ARCH}" = "aarch64" ]]; then ARCH="arm64"; fi \
    && if [[ "${BUILD_ARCH}" = "amd64" ]]; then ARCH="amd64"; fi \
    && if [[ "${BUILD_ARCH}" = "armhf" ]]; then ARCH="arm"; fi \
    && if [[ "${BUILD_ARCH}" = "armv7" ]]; then ARCH="arm"; fi \
    && if [[ "${BUILD_ARCH}" = "i386" ]]; then ARCH="386"; fi \
    \
    && curl -L -s \
         "https://github.com/restic/restic/releases/download/v0.9.6/restic_0.9.6_linux_${ARCH}.bz2" \
	 | tar zxvf - -C /usr/bin/ \
    && mv /usr/bin/restic_0.9.6_linux_${ARCH] /usr/bin/restic \
    && chmod a+x /usr/bin/restic \
    \
    && curl -L -s \
         "https://downloads.rclone.org/rclone-current-linux-${ARCH}.zip" \
	 | tar zxvf - -C /tmp/ \
    && mv /tmp/rclone-*-linux-*/rclone /usr/bin/rclone \
    && rm -r /tmp/rclone-*-linux-* \
    && chmod a+x /usr/bin/rclone \


    
# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="Restic Backup" \
    io.hass.description="Simple HA Backup to the cloud using Restic" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Matias Vidal <matias@m01.cl>" \
    org.label-schema.description="Simple HA Backup to the cloud using Restic" \
    org.label-schema.build-date=${BUILD_DATE} \
    org.label-schema.name="Restic Backup" \
    org.label-schema.schema-version="1.0" \
    org.label-schema.url="https://addons.community" \
    org.label-schema.usage="https://github.com/hassio-addons/addon-example/tree/master/README.md" \
    org.label-schema.vcs-ref=${BUILD_REF} \
    org.label-schema.vcs-url="https://github.com/hassio-addons/addon-example" \
    org.label-schema.vendor="Community Hass.io Addons"
