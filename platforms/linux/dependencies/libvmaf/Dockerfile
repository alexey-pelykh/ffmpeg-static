ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBVMAF_VERSION="2.3.1"
ARG LIBVMAF_URL="https://github.com/Netflix/vmaf/archive/refs/tags/v${LIBVMAF_VERSION}.tar.gz"
RUN curl -o "/tmp/libvmaf.tar.gz" -fsSL "$LIBVMAF_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libvmaf" \
    && tar -xzf "/tmp/libvmaf.tar.gz" -C "src/libvmaf" --strip-components 1 \
    && rm -rf "/tmp/libvmaf.tar.gz" \
    ;
RUN cd "src/libvmaf/libvmaf" \
    && meson setup \
        --prefix=/build \
        --libdir=lib \
        --default-library=static \
        --buildtype=release \
        ".meson" \
        -Dbuilt_in_models=true \
        -Denable_tests=false \
        -Denable_docs=false \
        -Denable_avx512=true \
        -Denable_float=true \
    && ninja -j$(nproc) -v -C ".meson" install \
    && ninja -j$(nproc) -v -C ".meson" clean \
    && sed -i 's/-lvmaf /-lvmaf -lstdc++ /' /build/lib/pkgconfig/libvmaf.pc \
    ;
