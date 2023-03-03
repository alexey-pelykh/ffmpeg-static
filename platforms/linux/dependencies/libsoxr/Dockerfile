ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBSOXR_VERSION="0.1.3"
ARG LIBSOXR_URL="https://downloads.sourceforge.net/project/soxr/soxr-${LIBSOXR_VERSION}-Source.tar.xz"
ARG LIBSOXR_SHA256="b111c15fdc8c029989330ff559184198c161100a59312f5dc19ddeb9b5a15889"
RUN curl -o "/tmp/libsoxr.tar.xz" -fsSL "$LIBSOXR_URL" \
    && echo "$LIBSOXR_SHA256  /tmp/libsoxr.tar.xz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libsoxr" \
    && tar -xJf "/tmp/libsoxr.tar.xz" -C "src/libsoxr" --strip-components 1 \
    && rm -rf "/tmp/libsoxr.tar.xz" \
    ;
RUN cd "src/libsoxr" \
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
