ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libgif:$TAG as libgif
FROM ${IMAGE_PREFIX}libjpeg-turbo:$TAG as libjpeg-turbo
FROM ${IMAGE_PREFIX}libpng:$TAG as libpng
FROM ${IMAGE_PREFIX}libtiff:$TAG as libtiff

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libgif /build .
COPY --from=libjpeg-turbo /build .
COPY --from=libpng /build .
COPY --from=libtiff /build .

ARG LIBWEBP_VERSION="1.3.0"
ARG LIBWEBP_URL="https://github.com/webmproject/libwebp/archive/v${LIBWEBP_VERSION}.tar.gz"
RUN curl -o "/tmp/libwebp.tar.gz" -fsSL "$LIBWEBP_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libwebp" \
    && tar -xzf "/tmp/libwebp.tar.gz" -C "src/libwebp" --strip-components 1 \
    && rm -rf "/tmp/libwebp.tar.gz" \
    ;
RUN cd "src/libwebp" \
    && ./autogen.sh \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
        --with-pic \
        --enable-libwebpmux \
        --disable-libwebpextras \
        --enable-libwebpdemux \
        --disable-sdl \
        --disable-gl \
        --enable-png \
        --enable-jpeg \
        --enable-tiff \
        --enable-gif \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
