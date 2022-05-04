ARG UbuntuVersion=22.04

FROM ubuntu:$UbuntuVersion
ARG UbuntuVersion=22.04
ARG GdbVersion=12.1
ARG BuildVersion=1

RUN apt update && apt install -y \
    build-essential \
    m4 \
    wget \
    xz-utils \
    libgmp-dev

RUN wget https://ftp.gnu.org/gnu/gdb/gdb-${GdbVersion}.tar.xz && \
    xz -d gdb-${GdbVersion}.tar.xz && \
    tar xf gdb-${GdbVersion}.tar

RUN cd gdb-${GdbVersion} && \
    ./configure && \
    make -j`nproc` && \
    make install-strip DESTDIR=/gdb-$GdbVersion-$BuildVersion-ubuntu-$UbuntuVersion

COPY DEBIAN/control.m4 /
RUN mkdir /gdb-$GdbVersion-$BuildVersion-ubuntu-$UbuntuVersion/DEBIAN
RUN m4 -P -DUBUNTUVERSION=$UbuntuVersion -DVERSION=$GdbVersion-$BuildVersion control.m4 > /gdb-$GdbVersion-$BuildVersion-ubuntu-$UbuntuVersion/DEBIAN/control
RUN dpkg-deb --build --root-owner-group /gdb-$GdbVersion-$BuildVersion-ubuntu-$UbuntuVersion