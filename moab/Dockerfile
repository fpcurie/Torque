FROM centos:7.3.1611

RUN yum install -y net-tools epel-release sssd-client openssh-clients mysql-connector-odbc && \
    cat /usr/share/zoneinfo/Europe/Paris > /etc/localtime && \
    sed -i 's/^initgroups.*/initgroups: files sss/g' /etc/nsswitch.conf

ADD https://github.com/Yelp/dumb-init/releases/download/v1.2.0/dumb-init_1.2.0_amd64 /
ADD rootfs /

ENV TORQUE_VER="6.1.0-3" MOAB_VER="9.1.0-1" PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/opt/moab/bin:/opt/moab/sbin" XDG_CONFIG_HOME="/opt/moab/etc"

RUN yum install -y supervisor torque-client-$TORQUE_VER torque-devel-$TORQUE_VER moab-$MOAB_VER clustershell && \
    chmod +x /dumb-init*

CMD ["/dumb-init_1.2.0_amd64", "/usr/bin/supervisord"] 
