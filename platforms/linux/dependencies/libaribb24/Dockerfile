ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libpng:$TAG as libpng

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libpng /build .

ARG LIBARIBB24_VERSION="1.0.3"
ARG LIBARIBB24_URL="https://github.com/nkoriyama/aribb24/archive/v${LIBARIBB24_VERSION}.tar.gz"
RUN curl -o "/tmp/libaribb24.tar.gz" -fsSL "$LIBARIBB24_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libaribb24" \
    && tar -xzf "/tmp/libaribb24.tar.gz" -C "src/libaribb24" --strip-components 1 \
    && rm -rf "/tmp/libaribb24.tar.gz" \
    ;
RUN cd "src/libaribb24" \
    && autoreconf -fiv \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
