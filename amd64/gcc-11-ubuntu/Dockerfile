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
    --disable-vtable-verify \
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
    --enable-checking=release \
    --build=x86_64-linux-gnu \
    --host=x86_64-linux-gnu \
    --target=x86_64-linux-gnu \
    --with-build-config=bootstrap-lto-lean \
    --enable-link-serialization=2
RUN make -j`nproc`
RUN make install-strip DESTDIR=/gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion
RUN mkdir -p /gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion/usr/share/gdb/auto-load/usr/lib64 && \
    mv /gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion/usr/lib64/libstdc++.so.6.0.29-gdb.py /gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion/usr/share/gdb/auto-load/usr/lib64/libstdc++.so.6.0.29-gdb.py

# Build aarch64-gcc-11
RUN apt install -y g++-aarch64-linux-gnu
RUN mkdir /aarch64-gcc-11
WORKDIR /aarch64-gcc-11
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
    --enable-checking=release \
    --build=x86_64-linux-gnu \
    --host=x86_64-linux-gnu \
    --target=aarch64-linux-gnu \
    --program-prefix=aarch64-linux-gnu- \
    --includedir=/usr/aarch64-linux-gnu/include \
    --with-build-config=bootstrap-lto-lean \
    --enable-link-serialization=2
RUN make -j`nproc`
RUN make install-strip DESTDIR=/gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion
RUN mkdir -p /gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion/usr/share/gdb/auto-load/usr/aarch64-linux-gnu/lib64 && \
    mv /gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion/usr/aarch64-linux-gnu/lib64/libstdc++.so.6.0.29-gdb.py /gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion/usr/share/gdb/auto-load/usr/aarch64-linux-gnu/lib64/libstdc++.so.6.0.29-gdb.py

# Generate deb
WORKDIR /
COPY DEBIAN/control.m4 /
RUN mkdir /gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion/DEBIAN
RUN m4 -P -DUBUNTUVERSION=$UbuntuVersion -DVERSION=$Gcc11Version-$BuildVersion control.m4 > /gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion/DEBIAN/control
RUN dpkg-deb --build --root-owner-group /gcc-$Gcc11Version-$BuildVersion-ubuntu-$UbuntuVersion