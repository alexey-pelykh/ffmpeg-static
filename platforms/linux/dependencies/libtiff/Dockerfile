ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libjpeg-turbo:$TAG as libjpeg-turbo
FROM ${IMAGE_PREFIX}libz:$TAG as libz

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libjpeg-turbo /build .
COPY --from=libz /build .

ARG LIBTIFF_VERSION="4.5.0"
ARG LIBTIFF_URL="https://gitlab.com/libtiff/libtiff/-/archive/v${LIBTIFF_VERSION}/libtiff-v${LIBTIFF_VERSION}.tar.gz"
RUN curl -o "/tmp/libtiff.tar.gz" -fsSL "$LIBTIFF_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libtiff" \
    && tar -xzf "/tmp/libtiff.tar.gz" -C "src/libtiff" --strip-components 1 \
    && rm -rf "/tmp/libtiff.tar.gz" \
    ;
RUN cd "src/libtiff" \
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
        -Dtiff-tools=OFF \
        -Dtiff-tests=OFF \
        -Dtiff-contrib=OFF \
        -Dtiff-docs=OFF \
        -Dtiff-deprecated=OFF \
        -Dtiff-install=OFF \
        .. \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
