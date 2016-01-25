FROM phusion/baseimage:latest
 
MAINTAINER roach@dopegoat.com

CMD ["/sbin/my_init"]
 
RUN apt-get update && apt-get upgrade -y -o Dpkg::Options::="--force-confold"
RUN apt-get install --no-install-recommends -y apache2-utils git git-core build-essential libssl-dev g++ sshfs libxml2-dev python2.7

RUN curl -sL https://deb.nodesource.com/setup_5.x | bash -
RUN apt-get install -y nodejs

RUN git clone https://github.com/c9/core.git /cloud9
WORKDIR /cloud9
RUN scripts/install-sdk.sh
RUN sed -i 's#127.0.0.1#0.0.0.0#' /cloud9/configs/standalone.js 

RUN mkdir -p /etc/my_init.d /workspace
VOLUME /workspace

RUN mkdir /etc/service/cloud9
RUN echo '#!/bin/sh' >/etc/service/cloud9/run
RUN echo 'export NODE_ENV="production"' >>/etc/service/cloud9/run
RUN echo 'export HOME=/root' >>/etc/service/cloud9/run
RUN echo 'cd /cloud9' >>/etc/service/cloud9/run
RUN echo 'exec /usr/bin/node /cloud9/server.js --listen 0.0.0.0 --port 8080 -w /workspace' >>/etc/service/cloud9/run
RUN chmod +x /etc/service/cloud9/run

RUN touch /etc/service/cron/down
 
EXPOSE 8080
 
