# syntax=docker/dockerfile:1

# This image should be built with: 
# docker build --tag onetagger:${VER}-{BASEOS} - < Dockerfile
# - $VER = version of onetagger used to build the image
# - $BASEOS = OS of the base image (this file currently uses ubuntu)

FROM ubuntu:20.04
## Server config
EXPOSE 36913/tcp
WORKDIR /data/music
ARG releaseurl='https://github.com/Marekkon5/onetagger/releases/download/1.7.0/OneTagger-linux.tar.gz'

RUN DEBIAN_FRONTEND=noninteractive apt-get update -yq \
    && DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
    libssl-dev \
    libsndfile1-dev \
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
    && chown 0:0 /usr/bin/onetagger
RUN ln -s ~/.config /config
## Post-download cleanup
# Remove curl
RUN apt-get purge -yq curl

CMD onetagger --server --expose --path /data/music