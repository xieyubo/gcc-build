ARG UbuntuVersion=20.04

FROM ubuntu:$UbuntuVersion

RUN apt update && apt install -y wget build-essential file flex libz-dev libzstd-dev

# Download gcc source code
ARG Gcc12Version=12.2.0
RUN wget https://gcc.gnu.org/pub/gcc/releases/gcc-$Gcc12Version/gcc-$Gcc12Version.tar.gz
RUN tar xf gcc-$Gcc12Version.tar.gz
RUN cd /gcc-$Gcc12Version && ./contrib/download_prerequisites

# Build x86_64-gcc-12
ARG BuildVersion=1
RUN mkdir /x86_64-gcc-12
WORKDIR /x86_64-gcc-12
RUN /gcc-$Gcc12Version/configure \
    --with-pkgversion="xyb-build-$BuildVersion https://github.com/xieyubo/gcc-build" \
    --enable-languages=c,c++ \
    --prefix=/usr \
    --with-gcc-major-version-only \
    --program-suffix=-12 \
    --enable-shared \
    --enable-linker-build-id \
    --libexecdir=/usr/lib \
    --without-included-gettext \
    --enable-threads=posix \
    --libdir=/usr/lib \
    --disable-nls \
    --enable-clocale=gnu \
    --enable-libstdcxx-backtrace=yes \
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
    --target=x86_64-linux-gnu
RUN make -j`nproc`

ARG UbuntuVersion=20.04

RUN make install-strip DESTDIR=/gcc-$Gcc12Version-$BuildVersion-ubuntu-$UbuntuVersion
RUN mkdir -p /gcc-$Gcc12Version-$BuildVersion-ubuntu-$UbuntuVersion/usr/share/gdb/auto-load/usr/lib64 && \
    mv /gcc-$Gcc12Version-$BuildVersion-ubuntu-$UbuntuVersion/usr/lib64/libstdc++.so.6.0.30-gdb.py /gcc-$Gcc12Version-$BuildVersion-ubuntu-$UbuntuVersion/usr/share/gdb/auto-load/usr/lib64/libstdc++.so.6.0.30-gdb.py

# Generate deb
WORKDIR /
COPY DEBIAN/control.m4 /
RUN mkdir /gcc-$Gcc12Version-$BuildVersion-ubuntu-$UbuntuVersion/DEBIAN
RUN m4 -P -DUBUNTUVERSION=$UbuntuVersion -DVERSION=$Gcc12Version-$BuildVersion control.m4 > /gcc-$Gcc12Version-$BuildVersion-ubuntu-$UbuntuVersion/DEBIAN/control
RUN dpkg-deb --build --root-owner-group /gcc-$Gcc12Version-$BuildVersion-ubuntu-$UbuntuVersion