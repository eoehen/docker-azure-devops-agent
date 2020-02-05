FROM ubuntu:18.04
LABEL MAINTAINER="BBT Software AG <devadmin@bbtsoftware.ch>"

# To make it easier for build and release pipelines to run apt-get,
# configure apt to not require confirmation (assume the -y argument by default)
ENV DEBIAN_FRONTEND=noninteractive
RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes

RUN mkdir /azp && \
    groupadd -r azp --gid=999 && \
    useradd -r -g azp --uid=999 --home-dir=/azp --shell=/bin/bash azp && \
    chown azp:azp /azp && \
    apt-get update

RUN apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        jq \
        git \
        iputils-ping \
        libcurl4 \
        libicu60 \
        libunwind8 \
        netcat \ 
        zip \
        unzip

# install dotnet 3.1
RUN wget -q https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb && \
    dpkg -i packages-microsoft-prod.deb && \
    apt-get update && \
    apt-get -y install dotnet-sdk-3.1

WORKDIR /azp

COPY docker-entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]

USER azp