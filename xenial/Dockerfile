FROM ubuntu:xenial-20180123

RUN apt update && \
    apt install -y apt-mirror && \
    apt autoremove -y && apt clean && rm -rf /var/lib/apt/lists* /tmp/* /var/tmp/* 

VOLUME ["/var/spool/apt-mirror"]

CMD ["watch", "-n", "86400", "apt-mirror"]

EXPOSE 80