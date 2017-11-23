FROM debian:jessie-slim as builder
MAINTAINER Patrick Kleindienst

RUN apt-get update && \
    apt-get install -y curl

ARG MESOS_VERSION=1.1.1
ARG DEBIAN_VERSION=debian8
ARG ARCH=amd64

RUN curl -o mesos.deb \ 
http://repos.mesosphere.com/debian/pool/main/m/mesos/\
mesos_${MESOS_VERSION}-2.0.1.${DEBIAN_VERSION}_${ARCH}.deb

ARG DOCKER_VERSION=17.09.0-ce
ARG DOCKER_ARCH=x86_64

RUN curl -o docker.tgz \ 
https://download.docker.com/linux/static/stable/${DOCKER_ARCH}/docker-${DOCKER_VERSION}.tgz && \
tar -xf docker.tgz

FROM debian:jessie-slim

COPY --from=builder /mesos.deb .
COPY --from=builder /docker/docker /usr/local/bin

RUN chmod +x /usr/local/bin/docker

RUN apt-get update && \
    apt-get install -y gdebi-core && \
    gdebi mesos.deb

