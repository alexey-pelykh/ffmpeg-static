ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBFRIBIDI_VERSION="1.0.12"
ARG LIBFRIBIDI_URL="https://github.com/fribidi/fribidi/releases/download/v${LIBFRIBIDI_VERSION}/fribidi-${LIBFRIBIDI_VERSION}.tar.xz"
ARG LIBFRIBIDI_SHA256="0cd233f97fc8c67bb3ac27ce8440def5d3ffacf516765b91c2cc654498293495"
RUN curl -o "/tmp/libfribidi.tar.xz" -fsSL "$LIBFRIBIDI_URL" \
    && echo "$LIBFRIBIDI_SHA256  /tmp/libfribidi.tar.xz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libfribidi" \
    && tar -xJf "/tmp/libfribidi.tar.xz" -C "src/libfribidi" --strip-components 1 \
    && rm -rf "/tmp/libfribidi.tar.xz" \
    ;
RUN cd "src/libfribidi" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
