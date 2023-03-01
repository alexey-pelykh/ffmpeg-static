ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBXAVS2_VERSION="1.4"
ARG LIBXAVS2_URL="https://github.com/pkuvcl/xavs2/archive/refs/tags/${LIBXAVS2_VERSION}.tar.gz"
RUN curl -o "/tmp/libxavs2.tar.gz" -fsSL "$LIBXAVS2_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libxavs2" \
    && tar -xzf "/tmp/libxavs2.tar.gz" -C "src/libxavs2" --strip-components 1 \
    && rm -rf "/tmp/libxavs2.tar.gz" \
    ;
RUN cd "src/libxavs2/build/linux" \
    && ./configure \
        --prefix=/build \
        --enable-static \
        --enable-pic \
        --disable-asm \
        --disable-cli \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
