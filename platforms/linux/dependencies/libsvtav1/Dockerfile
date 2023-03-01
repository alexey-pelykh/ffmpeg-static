ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBSVTAV1_VERSION="1.4.1"
ARG LIBSVTAV1_URL="https://gitlab.com/AOMediaCodec/SVT-AV1/-/archive/v${LIBSVTAV1_VERSION}/SVT-AV1-v${LIBSVTAV1_VERSION}.tar.bz2"
ARG LIBSVTAV1_SHA256="0e988582f315fe76c909accf5e7f81b975c5bd2b850ee760d8e9fac297f70b5d"
RUN curl -o "/tmp/libsvtav1.tar.bz2" -fsSL "$LIBSVTAV1_URL" \
    && echo "$LIBSVTAV1_SHA256  /tmp/libsvtav1.tar.bz2" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libsvtav1" \
    && tar -xjf "/tmp/libsvtav1.tar.bz2" -C "src/libsvtav1" --strip-components 1 \
    && rm -rf "/tmp/libsvtav1.tar.bz2" \
    ;
RUN cd "src/libsvtav1" \
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
        .. \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
