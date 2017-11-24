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

ARG DOCKER_VERSION=17.09.0-ce
ARG DOCKER_ARCH=x86_64
ARG DOCKER_TARBALL=docker-${DOCKER_VERSION}.tgz

RUN curl -o ${DOCKER_TARBALL} \ 
https://download.docker.com/linux/static/stable/${DOCKER_ARCH}/${DOCKER_TARBALL} && \
tar -xf ${DOCKER_TARBALL}

FROM debian:jessie-slim

COPY --from=builder /mesos.deb .
COPY --from=builder /docker/docker /usr/local/bin

RUN chmod +x /usr/local/bin/docker

# Workaround since installation of openjdk-7-jre-headless fails otherwise due to symlink issue.
# Details: https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=863199
RUN mkdir -p /usr/share/man/man1/

RUN apt-get update && \
    apt-get install -y gdebi-core && \
    gdebi -n mesos.deb

