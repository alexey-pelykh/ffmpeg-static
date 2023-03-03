ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBSNAPPY_VERSION="1.1.9"
ARG LIBSNAPPY_URL="https://github.com/google/snappy/archive/refs/tags/${LIBSNAPPY_VERSION}.tar.gz"
RUN curl -o "/tmp/libsnappy.tar.gz" -fsSL "$LIBSNAPPY_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libsnappy" \
    && tar -xzf "/tmp/libsnappy.tar.gz" -C "src/libsnappy" --strip-components 1 \
    && rm -rf "/tmp/libsnappy.tar.gz" \
    ;

RUN cd "src/libsnappy" \
    && mkdir ".cmake" \
    && cd ".cmake" \
    && cmake \
        -G"Unix Makefiles" \
        -DCMAKE_INSTALL_PREFIX:PATH=/build \
        -DBUILD_SHARED_LIBS=OFF \
        -DBUILD_STATIC_LIBS=ON \
        -DCMAKE_INSTALL_LIBDIR=lib \
        -DCMAKE_VERBOSE_MAKEFILE=ON \
        -DCMAKE_BUILD_TYPE=Release \
        -DSNAPPY_BUILD_TESTS=OFF \
        -DSNAPPY_BUILD_BENCHMARKS=OFF \
        .. \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
