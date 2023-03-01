ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBZVBI_VERSION="0.2.39"
ARG LIBZVBI_URL="https://github.com/zapping-vbi/zvbi/archive/refs/tags/v${LIBZVBI_VERSION}.tar.gz"
RUN curl -o "/tmp/libzvbi.tar.gz" -fsSL "$LIBZVBI_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libzvbi" \
    && tar -xzf "/tmp/libzvbi.tar.gz" -C "src/libzvbi" --strip-components 1 \
    && rm -rf "/tmp/libzvbi.tar.gz" \
    ;
RUN cd "src/libzvbi" \
    && ./autogen.sh \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
