ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base-python:3.12-alpine3.22
FROM ${BUILD_FROM}

ARG CET_URL=https://cybrotech.com/wp-content/uploads/CybroEdgeToolkit.zip
ARG CET_SHA256=f3bc060b16d614b2a73a91361666edb3f1e0b908f92ae7ce67fc6b1e376d16b7

ENV CET_HOME=/opt/cybrotech

# Download the Cybro Edge Toolkit and install its Python dependencies.
# WMI is Windows-only and therefore skipped.
RUN set -eux; \
    apk add --no-cache --virtual .build-deps \
        build-base \
        linux-headers; \
    mkdir -p "${CET_HOME}" /tmp/cet; \
    curl -fsSL -o /tmp/cet.zip "${CET_URL}"; \
    echo "${CET_SHA256}  /tmp/cet.zip" | sha256sum -c -; \
    unzip -q /tmp/cet.zip -d /tmp/cet; \
    mv /tmp/cet/app "${CET_HOME}/app"; \
    mv /tmp/cet/History.txt "${CET_HOME}/"; \
    grep -viE '^\s*wmi\b' "${CET_HOME}/app/requirements.txt" > /tmp/requirements.txt; \
    pip install --no-cache-dir -r /tmp/requirements.txt; \
    mkdir -p "${CET_HOME}/defaults"; \
    mv "${CET_HOME}/app/config.ini" "${CET_HOME}/app/data_logger.xml" "${CET_HOME}/defaults/"; \
    ln -s /config/config.ini "${CET_HOME}/app/config.ini"; \
    ln -s /config/data_logger.xml "${CET_HOME}/app/data_logger.xml"; \
    ln -s /data/log "${CET_HOME}/app/log"; \
    ln -s /data/alc "${CET_HOME}/app/alc"; \
    apk del --no-network .build-deps; \
    rm -rf /tmp/cet /tmp/cet.zip /tmp/requirements.txt

COPY rootfs /

WORKDIR ${CET_HOME}/app
