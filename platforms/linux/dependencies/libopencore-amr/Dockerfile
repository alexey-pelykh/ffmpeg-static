ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBOPENCORE_AMR_VERSION="0.1.6"
ARG LIBOPENCORE_AMR_URL="https://sourceforge.net/projects/opencore-amr/files/opencore-amr/opencore-amr-${LIBOPENCORE_AMR_VERSION}.tar.gz"
ARG LIBOPENCORE_AMR_SHA256="483eb4061088e2b34b358e47540b5d495a96cd468e361050fae615b1809dc4a1"
RUN curl -o "/tmp/libopencore-amr.tar.gz" -fsSL "$LIBOPENCORE_AMR_URL" \
    && echo "$LIBOPENCORE_AMR_SHA256  /tmp/libopencore-amr.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libopencore-amr" \
    && tar -xzf "/tmp/libopencore-amr.tar.gz" -C "src/libopencore-amr" --strip-components 1 \
    && rm -rf "/tmp/libopencore-amr.tar.gz" \
    ;
RUN cd "src/libopencore-amr" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
