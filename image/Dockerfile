# Python 3.6 Alpine image
FROM python:3.6-alpine

# Docker labels
LABEL maintainer="https://github.com/doucol" \
      description="Elastalert on Docker & Kubernetes"

# Directory holding configuration for Elastalert and Supervisor
ENV CONFIG_DIR /opt/config
# Elastalert rules directory
ENV RULES_DIR /opt/rules
# Elastalert timezone
ENV TZ America/Chicago

# Install dependencies
RUN apk update && apk add \
  rust \
  cvs \
  git \
  mercurial \
  ca-certificates \
  openssl-dev \
  libffi-dev \
  gcc \
  musl-dev \
  tzdata \
  openntpd \
  libmagic \
  wget \
  cargo

# Create directories. The /var/empty directory is used by openntpd.
RUN mkdir -p \
  "${CONFIG_DIR}" \
  "${RULES_DIR}" \
  /var/empty

# Install elastalert
RUN pip install --upgrade wheel setuptools
RUN pip3 install --upgrade pip setuptools wheel
RUN pip3 install elastalert==0.2.4

# Clean up.
RUN apk del \
  rust \
  cargo \
  musl-dev \
  gcc \
  openssl-dev \
  libffi-dev

# Clean cache
RUN rm -rf /var/cache/apk/*

# Define mount points.
VOLUME ["${CONFIG_DIR}","${RULES_DIR}"]

# Copy configuration file and rule example
COPY config/elastalert_config.yaml ${CONFIG_DIR}
COPY rules/example-rule.yaml ${RULES_DIR}

# Just for the record, which versions of things do we have
RUN python3 --version
RUN pip3 list

# Start Elastalert
CMD elastalert --verbose --config ${CONFIG_DIR}/elastalert_config.yaml
