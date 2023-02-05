[Root-docker](https://github.com/root-project/root-docker/blob/master/ubuntu/Dockerfile) has various parts

1. The base image is based out of ubuntu.

```Dockerfile
FROM ubuntu:22.04

LABEL maintainer.name="ROOT team"
LABEL maintainer.email="root-dev@cern.ch"

ENV LANG=C.UTF-8

ARG ROOT_BIN=root_v6.26.10.Linux-ubuntu22-x86_64-gcc11.3.tar.gz
```

2. ROOT installation

```Dockerfile
WORKDIR /opt

COPY packages packages

RUN apt-get update -qq \
	&& ln -sf /usr/share/zoneinfo/UTC /etc/localtime \
	&& apt-get -y install $(cat packages) wget\
	&& rm -rf /var/lib/apt/lists/*\
	&& wget https://root.cern/download/${ROOT_BIN} \
	&& tar -xzvf ${ROOT_BIN} \
	&& rm -f ${ROOT_BIN} \
	&& echo /opt/root/lib >> /etc/ld.so.conf \
	&& ldconfig
RUN yes | unminimize

ENV ROOTSYS /opt/root
ENV PATH $ROOTSYS/bin:$PATH
ENV PYTHONPATH $ROOTSYS/lib:$PYTHONPATH
ENV CLING_STANDARD_PCH none

CMD ["root", "-b"]
```
