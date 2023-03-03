ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libz:$TAG as libz

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libz /build .

ARG LIBOPENSSL_VERSION="1.1.1s"
ARG LIBOPENSSL_URL="https://www.openssl.org/source/openssl-${LIBOPENSSL_VERSION}.tar.gz"
ARG LIBOPENSSL_SHA256="c5ac01e760ee6ff0dab61d6b2bbd30146724d063eb322180c6f18a6f74e4b6aa"
RUN curl -o "/tmp/libopenssl.tar.gz" -fsSL "$LIBOPENSSL_URL" \
    && echo "$LIBOPENSSL_SHA256  /tmp/libopenssl.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libopenssl" \
    && tar -xzf "/tmp/libopenssl.tar.gz" -C "src/libopenssl" --strip-components 1 \
    && rm -rf "/tmp/libopenssl.tar.gz" \
    ;
RUN cd "src/libopenssl" \
    && ./config \
        --prefix=/build \
        --release \
        no-shared \
        threads \
        zlib \
    && make -j$(nproc) \
    && make install_sw \
    ;
