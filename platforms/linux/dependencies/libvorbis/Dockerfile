ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libogg:$TAG as libogg

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libogg /build .

ARG LIBVORBIS_VERSION="1.3.7"
ARG LIBVORBIS_URL="https://downloads.xiph.org/releases/vorbis/libvorbis-${LIBVORBIS_VERSION}.tar.gz"
ARG LIBVORBIS_SHA256="0e982409a9c3fc82ee06e08205b1355e5c6aa4c36bca58146ef399621b0ce5ab"
RUN curl -o "/tmp/libvorbis.tar.gz" -fsSL "$LIBVORBIS_URL" \
    && echo "$LIBVORBIS_SHA256  /tmp/libvorbis.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libvorbis" \
    && tar -xzf "/tmp/libvorbis.tar.gz" -C "src/libvorbis" --strip-components 1 \
    && rm -rf "/tmp/libvorbis.tar.gz" \
    ;
RUN cd "src/libvorbis" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
        --disable-oggtest \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
