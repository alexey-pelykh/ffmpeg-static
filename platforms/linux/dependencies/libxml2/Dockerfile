ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBXML2_VERSION="2.10.3"
ARG LIBXML2_URL="https://download.gnome.org/sources/libxml2/2.10/libxml2-${LIBXML2_VERSION}.tar.xz"
ARG LIBXML2_SHA256="5d2cc3d78bec3dbe212a9d7fa629ada25a7da928af432c93060ff5c17ee28a9c"
RUN curl -o "/tmp/libxml2.tar.xz" -fsSL "$LIBXML2_URL" \
    && echo "$LIBXML2_SHA256  /tmp/libxml2.tar.xz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libxml2" \
    && tar -xJf "/tmp/libxml2.tar.xz" -C "src/libxml2" --strip-components 1 \
    && rm -rf "/tmp/libxml2.tar.xz" \
    ;
RUN cd "src/libxml2" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
        --without-python \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
