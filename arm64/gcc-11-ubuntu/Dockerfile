ARG UbuntuVersion=20.04

FROM ubuntu:$UbuntuVersion

ARG UbuntuVersion=20.04
ARG Gcc11Version=11.2.0
ARG BuildVersion=1

# Download gcc source code
RUN apt update && apt install -y wget build-essential flex libz-dev libzstd-dev
RUN wget https://gcc.gnu.org/pub/gcc/releases/gcc-$Gcc11Version/gcc-$Gcc11Version.tar.gz
RUN tar xf gcc-$Gcc11Version.tar.gz
RUN cd /gcc-$Gcc11Version && ./contrib/download_prerequisites

# Build x86_64-gcc-11
RUN mkdir /x86_64-gcc-11
WORKDIR /x86_64-gcc-11
RUN /gcc-$Gcc11Version/configure \
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
    --disable-libquadmath \
    --disable-libquadmath-support \
    --enable-plugin \
    --enable-default-pie \
    --with-system-zlib \
    --enable-libphobos-checking=release \
    --with-target-system-zlib=auto \
    --disable-multilib \
    --enable-fix-cortex-a53-843419 \
    --disable-werror \
    --enable-checking=release \
    --build=aarch64-linux-gnu \
    --host=aarch64-linux-gnu \
    --target=aarch64-linux-gnu \
    --with-build-config=bootstrap-lto-lean \
    --enable-link-serialization=2
    
RUN make -j`nproc`
RUN make install-strip DESTDIR=/gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion-arm64
RUN mkdir -p /gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion-arm64/usr/share/gdb/auto-load/usr/lib64 && \
    mv /gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion-arm64/usr/lib64/libstdc++.so.6.0.29-gdb.py /gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion-arm64/usr/share/gdb/auto-load/usr/lib64/libstdc++.so.6.0.29-gdb.py

# Generate deb
WORKDIR /
COPY DEBIAN/control.m4 /
RUN mkdir /gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion-arm64/DEBIAN
RUN m4 -P -DUBUNTUVERSION=$UbuntuVersion -DVERSION=$Gcc11Version-$BuildVersion control.m4 > /gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion-arm64/DEBIAN/control
RUN dpkg-deb --build --root-owner-group /gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion-arm64