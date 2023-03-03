ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBSHINE_VERSION="3.1.1"
ARG LIBSHINE_URL="https://github.com/toots/shine/releases/download/${LIBSHINE_VERSION}/shine-${LIBSHINE_VERSION}.tar.gz"
ARG LIBSHINE_SHA256="58e61e70128cf73f88635db495bfc17f0dde3ce9c9ac070d505a0cd75b93d384"
RUN curl -o "/tmp/libshine.tar.gz" -fsSL "$LIBSHINE_URL" \
    && echo "$LIBSHINE_SHA256  /tmp/libshine.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libshine" \
    && tar -xzf "/tmp/libshine.tar.gz" -C "src/libshine" --strip-components 1 \
    && rm -rf "/tmp/libshine.tar.gz" \
    ;
RUN cd "src/libshine" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
        --with-pic \
        --disable-fast-install \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
