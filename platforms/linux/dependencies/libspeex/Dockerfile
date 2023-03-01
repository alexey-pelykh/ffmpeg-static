ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBSPEEX_VERSION="1.2.1"
ARG LIBSPEEX_URL="https://github.com/xiph/speex/archive/Speex-${LIBSPEEX_VERSION}.tar.gz"
RUN curl -o "/tmp/libspeex.tar.gz" -fsSL "$LIBSPEEX_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libspeex" \
    && tar -xzf "/tmp/libspeex.tar.gz" -C "src/libspeex" --strip-components 1 \
    && rm -rf "/tmp/libspeex.tar.gz" \
    ;
RUN cd "src/libspeex" \
    && ./autogen.sh \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
