FROM ubuntu:bionic-20180125

RUN apt update && \
    apt install -y cron apt-mirror && \
    apt autoremove -y && apt clean && rm -rf /var/lib/apt/lists* /tmp/* /var/tmp/* 

RUN echo "0 4 * * * root /usr/bin/apt-mirror > /var/spool/apt-mirror/cron.log" >> /etc/cron.d/apt-mirror

VOLUME ["/var/spool/apt-mirror"]

CMD ["cron", "-f"]