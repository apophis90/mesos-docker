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

FROM debian:jessie-slim

COPY --from=builder /mesos.deb .

RUN apt-get update && \
    apt-get install -y gdebi-core && \
	gdebi mesos.deb


