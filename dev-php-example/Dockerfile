# Match the version in current devilbox docker-compose.yml
FROM devilbox/php-fpm:7.4-work-0.122

ENV DENO_VERSION=1.6.2
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get -qq update \
        && apt-get -qq install -y --no-install-recommends curl ca-certificates unzip \
        && curl -fsSL https://github.com/denoland/deno/releases/download/v${DENO_VERSION}/deno-x86_64-unknown-linux-gnu.zip \
        --output deno.zip \
        && unzip deno.zip \
        && rm deno.zip \
        && chmod 777 deno \
        && mv deno /usr/bin/deno \
        && apt-get install -y xfce4 xfce4-goodies tightvncserver xfonts-base fonts-vlgothic firefox-esr && \
  apt-get clean \
        && apt-get -y -qq autoremove \
        && apt-get -qq clean \
        && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* 

ENV DENO_DIR /shared/httpd
ENV DENO_INSTALL_ROOT /usr/local

#Set envirnmental variable for display	
ENV DISPLAY 1:0

###
### Ports
###
### 5000: VNC
### 6000: X Window System
### 9000: PHP FPM

EXPOSE 5901
EXPOSE 6001
EXPOSE 9000

###
### Where to start inside the container
###
WORKDIR /shared/httpd

###
### Entrypoint
###
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
ENTRYPOINT ["/docker-entrypoint.sh"]
