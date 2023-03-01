ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libjpeg-turbo:$TAG as libjpeg-turbo

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libjpeg-turbo /build .

ARG LIBVPX_VERSION="1.12.0"
ARG LIBVPX_URL="https://github.com/webmproject/libvpx/archive/v${LIBVPX_VERSION}.tar.gz"
RUN curl -o "/tmp/libvpx.tar.gz" -fsSL "$LIBVPX_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libvpx" \
    && tar -xzf "/tmp/libvpx.tar.gz" -C "src/libvpx" --strip-components 1 \
    && rm -rf "/tmp/libvpx.tar.gz" \
    ;
RUN cd "src/libvpx" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
        --enable-vp9-highbitdepth \
        --disable-unit-tests \
        --disable-examples \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
