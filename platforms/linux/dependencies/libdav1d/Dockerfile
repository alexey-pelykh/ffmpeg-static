ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBDAV1D_VERSION="1.0.0"
ARG LIBDAV1D_URL="https://code.videolan.org/videolan/dav1d/-/archive/${LIBDAV1D_VERSION}/dav1d-${LIBDAV1D_VERSION}.tar.gz"
ARG LIBDAV1D_SHA256="047b8229511a82b5718a1d34c86c067b078efd02f602986d2ed09b23182ec136"
RUN curl -o "/tmp/libdav1d.tar.gz" -fsSL "$LIBDAV1D_URL" \
    && echo "$LIBDAV1D_SHA256  /tmp/libdav1d.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libdav1d" \
    && tar -xzf "/tmp/libdav1d.tar.gz" -C "src/libdav1d" --strip-components 1 \
    && rm -rf "/tmp/libdav1d.tar.gz" \
    ;
RUN cd "src/libdav1d" \
    && meson setup \
        --prefix=/build \
        --libdir=lib \
        --default-library=static \
        --buildtype release \
        ".meson" \
    && ninja -j$(nproc) -v -C ".meson" install \
    && ninja -j$(nproc) -v -C ".meson" clean \
    ;
