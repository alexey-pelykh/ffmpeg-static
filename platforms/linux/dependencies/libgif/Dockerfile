ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBGIF_VERSION="5.2.1"
ARG LIBGIF_URL="https://downloads.sourceforge.net/project/giflib/giflib-${LIBGIF_VERSION}.tar.gz"
ARG LIBGIF_SHA256="31da5562f44c5f15d63340a09a4fd62b48c45620cd302f77a6d9acf0077879bd"
RUN curl -o "/tmp/libgif.tar.gz" -fsSL "$LIBGIF_URL" \
    && echo "$LIBGIF_SHA256  /tmp/libgif.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libgif" \
    && tar -xzf "/tmp/libgif.tar.gz" -C "src/libgif" --strip-components 1 \
    && rm -rf "/tmp/libgif.tar.gz" \
    ;
RUN cd "src/libgif" \
    && make -j$(nproc) \
    && make -j$(nproc) install-include install-lib \
        PREFIX=/build \
    && make clean \
    && rm -rf /build/lib/libgif.so* \
    ;
