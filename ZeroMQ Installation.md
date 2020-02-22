## **ZeroMQ Installation**

#### libZmq Installation

    git clone https://github.com/zeromq/libzmq.git -b v4.3.0 --single-branch /tmp/libzmq
    mkdir /tmp/libzmq/build
    pushd /tmp/libzmq/build
    cmake -D WITH_PERF_TOOL=OFF -D ZMQ_BUILD_TESTS=OFF -D ENABLE_CPACK=OFF -D CMAKE_BUILD_TYPE=Release ..
    make -j4
    sudo make install
    popd

#### CPPZMQ Installation

    git clone https://github.com/zeromq/cppzmq.git -b v4.3.0 --single-branch /tmp/cppzmq
    mkdir /tmp/cppzmq/build
    pushd /tmp/cppzmq/build
    cmake -D CPPZMQ_BUILD_TESTS=OFF ..
    make -j4
    sudo make install
    popd