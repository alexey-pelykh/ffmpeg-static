ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBDC1394_VERSION="2.2.6"
ARG LIBDC1394_URL="https://downloads.sourceforge.net/project/libdc1394/libdc1394-2/${LIBDC1394_VERSION}/libdc1394-${LIBDC1394_VERSION}.tar.gz"
ARG LIBDC1394_SHA256="2b905fc9aa4eec6bdcf6a2ae5f5ba021232739f5be047dec8fe8dd6049c10fed"
RUN curl -o "/tmp/libdc1394.tar.gz" -fsSL "$LIBDC1394_URL" \
    && echo "$LIBDC1394_SHA256  /tmp/libdc1394.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libdc1394" \
    && tar -xzf "/tmp/libdc1394.tar.gz" -C "src/libdc1394" --strip-components 1 \
    && rm -rf "/tmp/libdc1394.tar.gz" \
    ;
RUN cd "src/libdc1394" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
