ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBLZMA_VERSION="5.4.1"
ARG LIBLZMA_URL="https://tukaani.org/xz/xz-${LIBLZMA_VERSION}.tar.gz"
ARG LIBLZMA_SHA256="e4b0f81582efa155ccf27bb88275254a429d44968e488fc94b806f2a61cd3e22"
RUN curl -o "/tmp/liblzma.tar.gz" -fsSL "$LIBLZMA_URL" \
    && echo "$LIBLZMA_SHA256  /tmp/liblzma.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/liblzma" \
    && tar -xzf "/tmp/liblzma.tar.gz" -C "src/liblzma" --strip-components 1 \
    && rm -rf "/tmp/liblzma.tar.gz" \
    ;
RUN cd "src/liblzma" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
