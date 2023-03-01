ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBMP3LAME_VERSION="3.100"
ARG LIBMP3LAME_URL="https://downloads.sourceforge.net/project/lame/lame/${LIBMP3LAME_VERSION}/lame-${LIBMP3LAME_VERSION}.tar.gz"
ARG LIBMP3LAME_SHA256="ddfe36cab873794038ae2c1210557ad34857a4b6bdc515785d1da9e175b1da1e"
RUN curl -o "/tmp/libmp3lame.tar.gz" -fsSL "$LIBMP3LAME_URL" \
    && echo "$LIBMP3LAME_SHA256  /tmp/libmp3lame.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libmp3lame" \
    && tar -xzf "/tmp/libmp3lame.tar.gz" -C "src/libmp3lame" --strip-components 1 \
    && rm -rf "/tmp/libmp3lame.tar.gz" \
    ;
RUN cd "src/libmp3lame" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
        --enable-nasm \
        --disable-gtktest \
        --disable-cpml \
        --disable-frontend \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
