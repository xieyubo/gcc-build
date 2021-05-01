# GCC Build Docker for Ubuntu

## Build gcc-11/aarch64-gcc-11 for Ubuntu 20.04

Run the following command to build gcc-11/aarch64-gcc-11:

    cd amd64/gcc-11-ubuntu-20.04
    docker build -t gcc-11:ubuntu-20.04 --build-arg Gcc11Version=11.1.0 --build-arg BuildVersion=0 .
    docker run --rm --entrypoint cat gcc-11:ubuntu-20.04 /gcc-11.1.0-0-ubuntu-20.04.deb > /tmp/gcc-11.1.0-0-ubuntu-20.04.deb

Now you can install it:

    sudo apt install -y /tmp/gcc-11.1.0-0-ubuntu-20.04.deb

Set gcc-11/aarch64-gcc-11 as high priority:

    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 11
    sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-10 10
    sudo update-alternatives --install /usr/bin/aarch64-linux-gnu-gcc aarch64-linux-gnu-gcc /usr/bin/aarch64-linux-gnu-gcc-11 11
    sudo update-alternatives --install /usr/bin/aarch64-linux-gnu-g++ aarch64-linux-gnu-g++ /usr/bin/aarch64-linux-gnu-g++-11 11

For convenience, add `/usr/lib64` to `LD_LIBRARY_PATH` environment:

    export LD_LIBRARY_PATH=/usr/lib64:$LD_LIBRARY_PATH
    echo "export LD_LIBRARY_PATH=\"/usr/lib64:\$LD_LIBRARY_PATH\"" >> ~/.bashrc
