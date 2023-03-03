ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBEXPAT_VERSION="2.5.0"
ARG LIBEXPAT_URL="https://github.com/libexpat/libexpat/releases/download/R_2_5_0/expat-${LIBEXPAT_VERSION}.tar.xz"
ARG LIBEXPAT_SHA256="ef2420f0232c087801abf705e89ae65f6257df6b7931d37846a193ef2e8cdcbe"
RUN curl -o "/tmp/libexpat.tar.xz" -fsSL "$LIBEXPAT_URL" \
    && echo "$LIBEXPAT_SHA256  /tmp/libexpat.tar.xz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libexpat" \
    && tar -xJf "/tmp/libexpat.tar.xz" -C "src/libexpat" --strip-components 1 \
    && rm -rf "/tmp/libexpat.tar.xz" \
    ;
RUN cd "src/libexpat" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
