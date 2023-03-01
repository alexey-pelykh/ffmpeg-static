ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBZ_VERSION="1.2.13"
ARG LIBZ_URL="https://www.zlib.net/zlib-${LIBZ_VERSION}.tar.gz"
ARG LIBZ_SHA256="b3a24de97a8fdbc835b9833169501030b8977031bcb54b3b3ac13740f846ab30"
RUN curl -o "/tmp/libz.tar.gz" -fsSL "$LIBZ_URL" \
    && echo "$LIBZ_SHA256  /tmp/libz.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libz" \
    && tar -xzf "/tmp/libz.tar.gz" -C "src/libz" --strip-components 1 \
    && rm -rf "/tmp/libz.tar.gz" \
    ;
RUN cd "src/libz" \
    && ./configure \
        --prefix=/build \
        --static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
