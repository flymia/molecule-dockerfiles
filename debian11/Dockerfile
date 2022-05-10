FROM debian:bullseye

ENV container docker
ENV LC_ALL C
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update \
    && apt-get install -y python3==3.9.2-3 sudo=1.9.5p2-3 bash=5.1-2+b3 ca-certificates=20210119 iproute2=5.10.0-4 python3-apt=2.2.1 aptitude=0.8.13-3 systemd=247.3-7 systemd-sysv=247.3-7 --no-install-recommends \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN rm -f /lib/systemd/system/multi-user.target.wants/* \
    /etc/systemd/system/*.wants/* \
    /lib/systemd/system/local-fs.target.wants/* \
    /lib/systemd/system/sockets.target.wants/*udev* \
    /lib/systemd/system/sockets.target.wants/*initctl* \
    /lib/systemd/system/sysinit.target.wants/systemd-tmpfiles-setup* \
    /lib/systemd/system/systemd-update-utmp*

RUN systemctl set-default multi-user.target

CMD [ "/lib/systemd/systemd", "log-level=info", "unit=sysinit.target" ]
