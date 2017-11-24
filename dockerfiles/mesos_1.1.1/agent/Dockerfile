FROM debian:jessie-slim as builder
MAINTAINER Patrick Kleindienst

ARG DOCKER_VERSION=17.09.0-ce
ARG DOCKER_ARCH=x86_64
ARG DOCKER_TARBALL=docker-${DOCKER_VERSION}.tgz 

RUN curl -o ${DOCKER_TARBALL} \
   https://download.docker.com/linux/static/stable/${DOCKER_ARCH}/${DOCKER_TARBALL} && \
   tar -xf ${DOCKER_TARBALL}


FROM pkleindienst/mesos:1.1.1

COPY --from=builder /docker/docker /usr/local/bin 

RUN chmod +x /usr/local/bin/docker

ENTRYPOINT ["mesos-agent"]
CMD ["--help"]

