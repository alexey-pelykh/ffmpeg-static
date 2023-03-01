ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBSAMPLERATE_VERSION="0.2.2"
ARG LIBSAMPLERATE_URL="https://github.com/libsndfile/libsamplerate/releases/download/${LIBSAMPLERATE_VERSION}/libsamplerate-${LIBSAMPLERATE_VERSION}.tar.xz"
ARG LIBSAMPLERATE_SHA256="3258da280511d24b49d6b08615bbe824d0cacc9842b0e4caf11c52cf2b043893"
RUN curl -o "/tmp/libsamplerate.tar.xz" -fsSL "$LIBSAMPLERATE_URL" \
    && echo "$LIBSAMPLERATE_SHA256  /tmp/libsamplerate.tar.xz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libsamplerate" \
    && tar -xJf "/tmp/libsamplerate.tar.xz" -C "src/libsamplerate" --strip-components 1 \
    && rm -rf "/tmp/libsamplerate.tar.xz" \
    ;
RUN cd "src/libsamplerate" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
