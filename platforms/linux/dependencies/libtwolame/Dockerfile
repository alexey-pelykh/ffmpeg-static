ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBTWOLAME_VERSION="0.4.0"
ARG LIBTWOLAME_URL="https://github.com/njh/twolame/releases/download/${LIBTWOLAME_VERSION}/twolame-${LIBTWOLAME_VERSION}.tar.gz"
ARG LIBTWOLAME_SHA256="cc35424f6019a88c6f52570b63e1baf50f62963a3eac52a03a800bb070d7c87d"
RUN curl -o "/tmp/libtwolame.tar.gz" -fsSL "$LIBTWOLAME_URL" \
    && echo "$LIBTWOLAME_SHA256  /tmp/libtwolame.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libtwolame" \
    && tar -xzf "/tmp/libtwolame.tar.gz" -C "src/libtwolame" --strip-components 1 \
    && rm -rf "/tmp/libtwolame.tar.gz" \
    ;
RUN cd "src/libtwolame" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
        --with-pic \
        --disable-sndfile \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
