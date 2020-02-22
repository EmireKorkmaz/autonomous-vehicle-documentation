## **Protocol Buffers Installation**

    sudo apt install autoconf automake libtool curl unzip protobuf-compiler
    git clone https://github.com/protocolbuffers/protobuf.git -b v3.9.1 --single-branch /tmp/protobuf
    cd /tmp/protobuf
    git submodule update --init --recursive
    ./autogen.sh
    ./configure --with-protoc=/usr/bin/protoc
    make -j4
    sudo make install