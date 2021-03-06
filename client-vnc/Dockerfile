ARG DOCKER_USERNAME
ARG ONEC_VERSION
FROM ${DOCKER_USERNAME}/onec-client:${ONEC_VERSION} as base

FROM debian:stretch-slim

COPY --from=base /opt /opt

ADD https://github.com/just-containers/s6-overlay/releases/download/v1.21.8.0/s6-overlay-amd64.tar.gz /tmp/

RUN apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    dirmngr \
    gnupg \
  && echo "deb http://http.debian.net/debian/ stretch main contrib non-free" > /etc/apt/sources.list \
  && echo "deb http://http.debian.net/debian/ stretch-updates main contrib non-free" >> /etc/apt/sources.list \
  && echo "deb http://security.debian.org/ stretch/updates main contrib non-free" >> /etc/apt/sources.list \
  && echo ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true | debconf-set-selections \
  && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 40976EAF437D05B5 \
  && echo "deb http://security.ubuntu.com/ubuntu xenial-security main" > /etc/apt/sources.list.d/xenial-security.list \
  && apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
      ca-certificates \
      git \
      locales \
      libwebkitgtk-3.0-0 \
      ttf-mscorefonts-installer \
      libfontconfig1 \
      libgsf-1-114 \
      libglib2.0-0 \
      libodbc1 \
      libpng12-0 \
      libmagickwand-6.q16-3 \
      dbus-x11 \
      at-spi2-core \
      procps \
      xvfb \
      x11vnc \
  && rm -rf  \
    /var/lib/apt/lists/* \
    /var/cache/debconf \
  && localedef -i ru_RU -c -f UTF-8 -A /usr/share/locale/locale.alias ru_RU.UTF-8
ENV LANG ru_RU.UTF-8

RUN tar xzf /tmp/s6-overlay-amd64.tar.gz -C / \
  && rm -rf /tmp/s6-overlay-amd64.tar.gz

RUN groupadd -r usr1cv8 --gid=2001 \
  && useradd -r -g usr1cv8 --uid=2001 --home-dir=/home/usr1cv8 --shell=/sbin/nologin usr1cv8 \
  && mkdir -p /home/usr1cv8/.1cv8 \
  && mkdir -p /home/usr1cv8/.1C/1cestart \
  && chown -R usr1cv8:usr1cv8 /home/usr1cv8

COPY ./configs/client-vnc/rootfs/ /

ENV DISPLAY=:0
ENV DISPLAY_WIDTH=1440
ENV DISPLAY_HEIGHT=900

EXPOSE 5900

ENTRYPOINT ["/init"]
