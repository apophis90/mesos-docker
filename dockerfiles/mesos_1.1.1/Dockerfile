FROM debian:jessie-slim as builder
MAINTAINER Patrick Kleindienst

RUN apt-get update && \
    apt-get install -y curl

ARG MESOS_VERSION=1.1.1
ARG DEBIAN_VERSION=debian8
ARG ARCH=amd64
ARG MESOS_DEB=mesos_${MESOS_VERSION}-2.0.1.${DEBIAN_VERSION}_${ARCH}.deb
ARG MESOS_FINAL_NAME=mesos.deb
ARG MESOS_CHECKSUM="84556d00e6fb5b693c73592dc52b7ce180731fe262efb621bd3baae41a44ada7  ${MESOS_FINAL_NAME}"

RUN curl -o ${MESOS_FINAL_NAME} \ 
http://repos.mesosphere.com/debian/pool/main/m/mesos/${MESOS_DEB} && \
echo "$MESOS_CHECKSUM" | sha256sum -c - && \
if [ "$?" != "0" ]; then exit 1; fi


FROM debian:jessie-slim

COPY --from=builder /mesos.deb .

# Workaround since installation of openjdk-7-jre-headless fails otherwise due to symlink issue.
# Details: https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=863199
RUN mkdir -p /usr/share/man/man1/

# libcurl4 must explicitly be installed since APT installs version 3 but Mesos is 
# linked against version 4.
RUN apt-get update && \
    apt-get install -y \
    gdebi-core \
    libcurl4-nss-dev && \
    gdebi -n mesos.deb

