ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBOPUS_VERSION="1.3.1"
ARG LIBOPUS_URL="https://archive.mozilla.org/pub/opus/opus-${LIBOPUS_VERSION}.tar.gz"
ARG LIBOPUS_SHA256="65b58e1e25b2a114157014736a3d9dfeaad8d41be1c8179866f144a2fb44ff9d"
RUN curl -o "/tmp/libopus.tar.gz" -fsSL "$LIBOPUS_URL" \
    && echo "$LIBOPUS_SHA256  /tmp/libopus.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libopus" \
    && tar -xzf "/tmp/libopus.tar.gz" -C "src/libopus" --strip-components 1 \
    && rm -rf "/tmp/libopus.tar.gz" \
    ;
RUN cd "src/libopus" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
        --disable-extra-programs \
        --disable-doc \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
