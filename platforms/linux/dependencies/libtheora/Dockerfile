ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libogg:$TAG as libogg

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libogg /build .

ARG LIBTHEORA_VERSION="1.1.1"
ARG LIBTHEORA_URL="https://downloads.xiph.org/releases/theora/libtheora-${LIBTHEORA_VERSION}.tar.bz2"
ARG LIBTHEORA_SHA256="b6ae1ee2fa3d42ac489287d3ec34c5885730b1296f0801ae577a35193d3affbc"
RUN curl -o "/tmp/libtheora.tar.bz2" -fsSL "$LIBTHEORA_URL" \
    && echo "$LIBTHEORA_SHA256  /tmp/libtheora.tar.bz2" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libtheora" \
    && tar -xjf "/tmp/libtheora.tar.bz2" -C "src/libtheora" --strip-components 1 \
    && rm -rf "/tmp/libtheora.tar.bz2" \
    ;
RUN cd "src/libtheora" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
        --build=$(arch)-unknown-linux-gnu \
        --disable-examples \
        --disable-oggtest \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
