# GCC Build Docker for Ubuntu

## GCC 12

### Build gcc-12/amd64 for Ubuntu 20.04

Run the following command to build gcc-12/amd64:

    cd amd64/gcc-12-ubuntu
    docker build -t gcc-12.2.0:ubuntu-20.04 .
    docker run --rm --entrypoint cat gcc-12.2.0:ubuntu-20.04 /gcc-12.2.0-1-ubuntu-20.04.deb > /tmp/gcc-12.2.0-1-ubuntu-20.04.deb

### Install

Now you can install it:

    sudo apt install -y /tmp/gcc-12.2.0-1-ubuntu-20.04.deb

Set gcc-12 as high priority:

    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-12 12
    sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-12 12

For convenience, add `/usr/lib64` to `LD_LIBRARY_PATH` environment:

    export LD_LIBRARY_PATH=/usr/lib64:$LD_LIBRARY_PATH
    echo "export LD_LIBRARY_PATH=\"/usr/lib64:\$LD_LIBRARY_PATH\"" >> ~/.bashrc

## GCC 11

### Build gcc-11/aarch64-gcc-11 for Ubuntu 20.04

Run the following command to build gcc-11/aarch64-gcc-11:

    cd amd64/gcc-11-ubuntu
    docker build -t gcc-11.2:ubuntu-20.04 --build-arg UbuntuVersion=20.04 .
    docker run --rm --entrypoint cat gcc-11.2:ubuntu-20.04 /gcc-11.2.0-1-ubuntu-20.04.deb > /tmp/gcc-11.2.0-1-ubuntu-20.04.deb

### Build gcc-11/aarch64-gcc-11 for Ubuntu 18.04

Run the following command to build gcc-11/aarch64-gcc-11:

    cd amd64/gcc-11-ubuntu
    docker build -t gcc-11.2:ubuntu-18.04 --build-arg UbuntuVersion=18.04 .
    docker run --rm --entrypoint cat gcc-11.2:ubuntu-18.04 /gcc-11.2.0-1-ubuntu-18.04.deb > /tmp/gcc-11.2.0-1-ubuntu-18.04.deb

### Install

Now you can install it:

    sudo apt install -y /tmp/gcc-11.2.0-1-ubuntu-20.04.deb

Set gcc-11/aarch64-gcc-11 as high priority:

    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 11
    sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-11 11
    sudo update-alternatives --install /usr/bin/aarch64-linux-gnu-gcc aarch64-linux-gnu-gcc /usr/bin/aarch64-linux-gnu-gcc-11 11
    sudo update-alternatives --install /usr/bin/aarch64-linux-gnu-g++ aarch64-linux-gnu-g++ /usr/bin/aarch64-linux-gnu-g++-11 11

For convenience, add `/usr/lib64` to `LD_LIBRARY_PATH` environment:

    export LD_LIBRARY_PATH=/usr/lib64:$LD_LIBRARY_PATH
    echo "export LD_LIBRARY_PATH=\"/usr/lib64:\$LD_LIBRARY_PATH\"" >> ~/.bashrc