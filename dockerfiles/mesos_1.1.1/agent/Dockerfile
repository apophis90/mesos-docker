FROM debian:jessie-slim as builder
MAINTAINER Patrick Kleindienst

RUN apt-get update && \
    apt-get install -y  curl 

ARG DOCKER_VERSION=17.09.0-ce
ARG DOCKER_ARCH=x86_64
ARG DOCKER_TARBALL=docker-${DOCKER_VERSION}.tgz 

RUN curl -o ${DOCKER_TARBALL} \
    https://download.docker.com/linux/static/stable/${DOCKER_ARCH}/${DOCKER_TARBALL} && \
    tar -xf ${DOCKER_TARBALL}


FROM pkleindienst/mesos:1.1.1

COPY --from=builder /docker/docker /usr/local/bin 

RUN chmod +x /usr/local/bin/docker

RUN echo deb http://http.debian.net/debian jessie-backports main > /etc/apt/sources.list.d/jessie-backports.list && \
    apt-get update && \
    apt-get -t jessie-backports -y install libssl1.0.0

ENTRYPOINT ["mesos-agent"]
