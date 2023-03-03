ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBAVISYNTH_VERSION="3.7.2"
ARG LIBAVISYNTH_URL="https://github.com/AviSynth/AviSynthPlus/archive/refs/tags/v${LIBAVISYNTH_VERSION}.tar.gz"
RUN curl -o "/tmp/libavisynth.tar.gz" -fsSL "$LIBAVISYNTH_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libavisynth" \
    && tar -xzf "/tmp/libavisynth.tar.gz" -C "src/libavisynth" --strip-components 1 \
    && rm -rf "/tmp/libavisynth.tar.gz" \
    ;
RUN cd "src/libavisynth" \
    && mkdir ".cmake" \
    && cd ".cmake" \
    && cmake \
        -G"Unix Makefiles" \
        -DCMAKE_INSTALL_PREFIX:PATH=/build \
        -DBUILD_SHARED_LIBS=OFF \
        -DBUILD_STATIC_LIBS=ON \
        -DCMAKE_INSTALL_LIBDIR=lib \
        -DCMAKE_VERBOSE_MAKEFILE=ON \
        -DCMAKE_BUILD_TYPE=Release \
        .. \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
