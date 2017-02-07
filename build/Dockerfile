FROM build:1.2

ENV VERSION=6.1.0 \
    RELEASE=3

# Hwloc & torque
RUN mkdir /sources && \
    wget -qO- -O tmp.tar.gz http://storageserver/Torque/torque-$VERSION.tar.gz && \
    tar -C /sources -zxf tmp.tar.gz && \
    rm tmp.tar.gz

RUN cd /sources/torque-$VERSION && \
#    ./autogen.sh && \
    ./configure --disable-gui \
                --prefix=/usr \
                --libdir=/usr/lib64 \
                --with-sched=no \
                --enable-cgroups \
                --disable-daemons \
                --with-check \
                --with-rcp=scp \
                --with-hwloc-path=/usr \
                --with-pam \
                --enable-drmaa \
                --with-default-server="torque6" && \
    make && make install && make packages && \
    mkdir /torque-{server,client,mom,devel,doc,pam,drmaa} && \
    ./torque-package-server-linux-x86_64.sh --install --destdir /torque-server && \
    ./torque-package-clients-linux-x86_64.sh --install --destdir /torque-client && \
    ./torque-package-mom-linux-x86_64.sh --install --destdir /torque-mom && \
    ./torque-package-devel-linux-x86_64.sh --install --destdir /torque-devel && \
    ./torque-package-doc-linux-x86_64.sh --install --destdir /torque-doc && \
    ./torque-package-pam-linux-x86_64.sh --install --destdir /torque-pam && \
    ./torque-package-drmaa-linux-x86_64.sh --install --destdir /torque-drmaa && \
    mkdir -p /torque-{server,client,mom}/etc/systemd/system && \
    cp contrib/systemd/pbs_server.service /torque-server/etc/systemd/system && \
    cp contrib/systemd/trqauthd.service /torque-client/etc/systemd/system && \
    cp contrib/systemd/pbs_mom.service /torque-mom/etc/systemd/system

ENV FPMOPTS="--license \"OpenPBS and TORQUEv1.1\" --url \"http://www.adaptivecomputing.com\" --version $VERSION --iteration $RELEASE"

RUN mkdir /rpms  && cd /rpms && \
    fpm -s dir -t rpm --rpm-use-file-permissions -C /torque-server --name torque-server -d hwloc --description "Torque server" && \
    fpm -s dir -t rpm --rpm-use-file-permissions -C /torque-client --name torque-client -d hwloc --description "Torque client" && \
    fpm -s dir -t rpm --rpm-use-file-permissions -C /torque-mom --name torque-mom -d hwloc --description "Torque mom" && \
    fpm -s dir -t rpm --rpm-use-file-permissions -C /torque-devel --name torque-devel --description "Torque devel" && \
    fpm -s dir -t rpm --rpm-use-file-permissions -C /torque-doc --name torque-doc --description "Torque doc" && \
    fpm -s dir -t rpm --rpm-use-file-permissions -C /torque-pam --name torque-pam --description "Torque pam" && \
    fpm -s dir -t rpm --rpm-use-file-permissions -C /torque-drmaa --name torque-drmaa --description "Torque drmaa"

