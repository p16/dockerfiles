FROM ubuntu:trusty
MAINTAINER elprans@sprymix.com

ENV APPDIR /srv/thumbor
ENV THUMBOR_VERSION 4.4.1
ENV THUMBOR_ENGINE graphicsmagick
ENV GRAPHICSMAGICK_ENGINE_VERSION 0.1.1

RUN mkdir -p /setup
ADD setup /setup
ADD config /setup/config
ADD init /init

ENV LANG en_US.UTF-8

RUN DEBIAN_FRONTEND=noninteractive \
    apt-get update && apt-get install --no-install-recommends -y language-pack-en-base && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y patch wget unzip python-dev python-pip && \
    DEBIAN_FRONTEND=noninteractive /setup/install && \
    DEBIAN_FRONTEND=noninteractive /setup/configure && \
    apt-get remove --purge patch wget unzip gcc gcc-4.8 perl -y && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /tmp/* && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    find /var/log -type f | while read f; do echo -ne '' > $f; done;

EXPOSE 8888

VOLUME ["/etc/persistent-conf"]
VOLUME ["/srv/thumbor/storage"]
VOLUME ["/dev/log"]

ENTRYPOINT ["/init"]
CMD ["start"]