# syntax=docker/dockerfile:1

# This image should be built with: 
# docker build --tag onetagger:${VER}-{BASEOS} - < Dockerfile
# - $VER = version of onetagger used to build the image
# - $BASEOS = OS of the base image (this file currently uses ubuntu)

FROM ubuntu:20.04

RUN apt update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
    libsndfile1-dev \
    libwebkit2gtk-4.0-dev \
    libasound2-dev \
    curl \
    tar \
    ca-certificates

# Update the CA certificates
RUN update-ca-certificates --fresh

EXPOSE 36912/tcp

WORKDIR /data

ARG releaseurl='https://github.com/Marekkon5/onetagger/releases/download/1.7.0/OneTagger-linux.tar.gz'
# `chown` is executed in the same RUN directive to save space
RUN curl -sL ${releaseurl} | tar xzvs && chown -R 1000:1000 ./* 
RUN 
CMD ./onetagger --server --expose