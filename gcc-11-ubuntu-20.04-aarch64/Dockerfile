FROM ubuntu:20.04

ARG GccSnapshotVersion

RUN dpkg --add-architecture arm64
RUN cat /etc/apt/sources.list | sed 's/^deb http:/deb \[arch=amd64\] http:/g' > /tmp/sources.list
RUN cat /etc/apt/sources.list | sed 's/^deb http:[^ ]*/deb \[arch=arm64\] http:\/\/ports\.ubuntu\.com\/ubuntu-ports\//g' >> /tmp/sources.list
RUN rm /etc/apt/sources.list
RUN mv /tmp/sources.list /etc/apt/sources.list
RUN apt-get update && apt install -y wget build-essential g++-aarch64-linux-gnu flex libz-dev
RUN wget https://gcc.gnu.org/pub/gcc/snapshots/$GccSnapshotVersion/gcc-$GccSnapshotVersion.tar.xz
RUN tar xf gcc-$GccSnapshotVersion.tar.xz

WORKDIR /gcc-$GccSnapshotVersion
RUN ./contrib/download_prerequisites

RUN mkdir /gcc-out
WORKDIR /gcc-out
RUN /gcc-$GccSnapshotVersion/configure \
    --enable-languages=c,c++ \
    --prefix=/usr \
    --with-gcc-major-version-only \
    --program-suffix=-11 \
    --enable-shared \
    --enable-linker-build-id \
    --libexecdir=/usr/lib \
    --without-included-gettext \
    --enable-threads=posix \
    --libdir=/usr/lib \
    --disable-nls \
    --with-sysroot=/ \
    --enable-clocale=gnu \
    --enable-libstdcxx-debug \
    --enable-libstdcxx-time=yes \
    --with-default-libstdcxx-abi=new \
    --enable-gnu-unique-object \
    --disable-libquadmath \
    --disable-libquadmath-support \
    --enable-plugin \
    --enable-default-pie \
    --with-system-zlib \
    --enable-libphobos-checking=release \
    --without-target-system-zlib \
    --enable-fix-cortex-a53-843419 \
    --disable-werror \
    --enable-checking=yes,extra,rtl \
    --build=x86_64-linux-gnu \
    --host=x86_64-linux-gnu \
    --target=aarch64-linux-gnu \
    --program-prefix=aarch64-linux-gnu- \
    --includedir=/usr/aarch64-linux-gnu/include \
    --with-build-config=bootstrap-lto-lean \
    --enable-link-serialization=2
RUN make -j`nproc`
RUN make install-strip DESTDIR=/gcc-$GccSnapshotVersion-0-ubuntu-20.04-aarch64

WORKDIR /
COPY DEBIAN/control.m4 /
RUN mkdir /gcc-$GccSnapshotVersion-0-ubuntu-20.04-aarch64/DEBIAN
RUN m4 -P -DVERSION=$GccSnapshotVersion-0 control.m4 > /gcc-$GccSnapshotVersion-0-ubuntu-20.04-aarch64/DEBIAN/control
RUN dpkg-deb --build --root-owner-group /gcc-$GccSnapshotVersion-0-ubuntu-20.04-aarch64