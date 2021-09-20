build-all-platforms:
    BUILD --platform=linux/arm64 --platform=linux/amd64 +build

builder:
    FROM debian:buster

    ENV DEBIAN_FRONTEND noninteractive
    ENV DEBCONF_NONINTERACTIVE_SEEN true

    RUN apt-get update && apt-get -y install \
        make \
        zlib1g-dev \
        libssl-dev \
        gperf \
        cmake \
        clang \
        libc++-dev \
        libc++abi-dev \
        && rm -rf /var/lib/apt/lists/*

code:
    FROM base


build:
    FROM +builder

    ARG TARGETARCH

    WORKDIR /build

    COPY ./ /code/

    RUN CXXFLAGS="-stdlib=libc++" \
        CC=/usr/bin/clang \
        CXX=/usr/bin/clang++ \
        cmake \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX:PATH=/tdlib \
        -S /code \
        -B .

    RUN cmake --build . --target install

    SAVE ARTIFACT /tdlib/lib/libtdjson.so AS LOCAL build/${TARGETARCH}/libtdjson.so
