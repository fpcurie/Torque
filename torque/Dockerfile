FROM centos:7.3.1611

RUN yum install -y net-tools epel-release sssd-client && \
    cat /usr/share/zoneinfo/Europe/Paris > /etc/localtime && \
    sed -i 's/^initgroups.*/initgroups: files sss/g' /etc/nsswitch.conf

https://github.com/Yelp/dumb-init/releases/download/v1.2.0/dumb-init_1.2.0_amd64 /
ADD rootfs /

ENV TORQUE_VER="6.1.0-3"

RUN yum install -y supervisor torque-client-$TORQUE_VER torque-server-$TORQUE_VER && \
    chmod +x /dumb-init*

CMD ["/dumb-init_1.2.0_amd64", "/usr/bin/supervisord"] 
