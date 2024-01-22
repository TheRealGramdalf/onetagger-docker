# syntax=docker/dockerfile:1

# This image should be built with: 
# docker build --build-arg=releaseurl='https://url.tld' --tag onetagger:${VER}-{BASEOS} - < Dockerfile
# - $VER = version of onetagger used to build the image
# - $BASEOS = OS of the base image (this file currently uses ubuntu)

FROM ubuntu:22.04
## Server config
EXPOSE 36913/tcp
WORKDIR /data/music
ARG releaseurl=''https://1t.marekkon5.workers.dev/artifact/a30d1b7/onetagger-linux-cli 

RUN DEBIAN_FRONTEND=noninteractive apt-get update -yq \
    && DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
    libasound2-dev \
    curl \
    ca-certificates \
    tar \
    && apt-get clean -yq

# Update the CA certificates
RUN update-ca-certificates --fresh


# `chown` is executed in the same RUN directive (layer) to save space
# Stream the zipped output directly to prevent writing intermediate files to disk. zcat (alias for: gzip) is part of coreutils
RUN curl -sL ${releaseurl} | zcat | tar xzvs -C /usr/bin/ \
    && chown 0:0 /usr/bin/onetagger-cli
RUN ln -s ~/.config /config
## Post-download cleanup
# Remove curl
RUN apt-get purge -yq curl

CMD onetagger-cli server --expose --path /data/music
