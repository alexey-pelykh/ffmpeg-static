ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libpython:$TAG as libpython
FROM ${IMAGE_PREFIX}libzimg:$TAG as libzimg

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libpython /build .
COPY --from=libzimg /build .

ARG LIBVAPOURSYNTH_VERSION="R61"
ARG LIBVAPOURSYNTH_URL="https://github.com/vapoursynth/vapoursynth/archive/refs/tags/${LIBVAPOURSYNTH_VERSION}.tar.gz"
RUN curl -o "/tmp/libvapoursynth.tar.gz" -fsSL "$LIBVAPOURSYNTH_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libvapoursynth" \
    && tar -xzf "/tmp/libvapoursynth.tar.gz" -C "src/libvapoursynth" --strip-components 1 \
    && rm -rf "/tmp/libvapoursynth.tar.gz" \
    ;
# NOTE: this is needed to ensure correct python and cython are found first
ENV PATH="/build/bin:$PATH"
RUN cd "src/libvapoursynth" \
    && ./autogen.sh \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
