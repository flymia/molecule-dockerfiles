FROM rockylinux/rockylinux:8
ENV container=docker

# Install systemd -- See https://hub.docker.com/_/centos/
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

# Install requirements.
RUN yum -y install rpm dnf-plugins-core \
 && yum -y update \
 && yum -y config-manager --set-enabled powertools \
 && yum -y install \
      epel-release \
      initscripts \
      sudo \
      which \
      hostname \
      python3 \
 && yum clean all

# Disable requiretty.
RUN sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers

#CMD ["/usr/lib/systemd/systemd"]
CMD [ "/lib/systemd/systemd", "log-level=info", "unit=sysinit.target" ]