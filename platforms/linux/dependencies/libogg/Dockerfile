ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBOGG_VERSION="1.3.5"
ARG LIBOGG_URL="https://downloads.xiph.org/releases/ogg/libogg-${LIBOGG_VERSION}.tar.gz"
ARG LIBOGG_SHA256="0eb4b4b9420a0f51db142ba3f9c64b333f826532dc0f48c6410ae51f4799b664"
RUN curl -o "/tmp/libogg.tar.gz" -fsSL "$LIBOGG_URL" \
    && echo "$LIBOGG_SHA256  /tmp/libogg.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libogg" \
    && tar -xzf "/tmp/libogg.tar.gz" -C "src/libogg" --strip-components 1 \
    && rm -rf "/tmp/libogg.tar.gz" \
    ;
RUN cd "src/libogg" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
