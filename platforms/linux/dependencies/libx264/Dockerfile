ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBX264_VERSION="baee400fa9ced6f5481a728138fed6e867b0ff7f"
ARG LIBX264_URL="https://code.videolan.org/videolan/x264/-/archive/${LIBX264_VERSION}/x264-${LIBX264_VERSION}.tar.gz"
RUN curl -o "/tmp/libx264.tar.gz" -fsSL "$LIBX264_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libx264" \
    && tar -xzf "/tmp/libx264.tar.gz" -C "src/libx264" --strip-components 1 \
    && rm -rf "/tmp/libx264.tar.gz" \
    ;
RUN cd "src/libx264" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
        --enable-pic \
        --disable-cli \
        --disable-lavf \
        --disable-swscale \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
