FROM ubuntu:20.04

ARG GccSnapshotVersion

RUN apt update && apt install -y wget build-essential flex libz-dev libzstd-dev
RUN wget https://gcc.gnu.org/pub/gcc/snapshots/$GccSnapshotVersion/gcc-$GccSnapshotVersion.tar.xz
RUN tar xf gcc-$GccSnapshotVersion.tar.xz

WORKDIR /gcc-$GccSnapshotVersion
RUN ./contrib/download_prerequisites
RUN ./configure \
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
    --enable-bootstrap \
    --enable-clocale=gnu \
    --enable-libstdcxx-debug \
    --enable-libstdcxx-time=yes \
    --with-default-libstdcxx-abi=new \
    --enable-gnu-unique-object \
    --disable-vtable-verify \
    --disable-libsanitizer \
    --enable-plugin \
    --enable-default-pie \
    --with-system-zlib \
    --enable-libphobos-checking=release \
    --with-target-system-zlib=auto \
    --disable-werror \
    --enable-cet \
    --with-abi=m64 \
    --disable-multilib \
    --with-tune=generic \
    --without-cuda-driver \
    --enable-checking=yes,extra,rtl \
    --build=x86_64-linux-gnu \
    --host=x86_64-linux-gnu \
    --target=x86_64-linux-gnu \
    --with-build-config=bootstrap-lto-lean \
    --enable-link-serialization=2
RUN make -j`nproc`
RUN make install-strip DESTDIR=/gcc-$GccSnapshotVersion-0-ubuntu-20.04-x86_64

WORKDIR /
COPY DEBIAN/control.m4 /
RUN mkdir /gcc-$GccSnapshotVersion-0-ubuntu-20.04-x86_64/DEBIAN
RUN m4 -P -DVERSION=$GccSnapshotVersion-0 control.m4 > /gcc-$GccSnapshotVersion-0-ubuntu-20.04-x86_64/DEBIAN/control
RUN dpkg-deb --build --root-owner-group /gcc-$GccSnapshotVersion-0-ubuntu-20.04-x86_64