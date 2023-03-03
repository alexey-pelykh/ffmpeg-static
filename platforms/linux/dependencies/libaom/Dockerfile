ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libjpeg-turbo:$TAG as libjpeg-turbo
FROM ${IMAGE_PREFIX}libvmaf:$TAG as libvmaf

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libjpeg-turbo /build .
COPY --from=libvmaf /build .

# NOTE: bcfe6fbfed315f83ee8a95465c654ee8078dbff9 matches 3.5.0
ARG LIBAOM_VERSION="bcfe6fbfed315f83ee8a95465c654ee8078dbff9"
ARG LIBAOM_URL="https://aomedia.googlesource.com/aom/+archive/${LIBAOM_VERSION}.tar.gz"
RUN curl -o "/tmp/libaom.tar.gz" -fsSL "$LIBAOM_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libaom" \
    && tar -xzf "/tmp/libaom.tar.gz" -C "src/libaom" \
    && rm -rf "/tmp/libaom.tar.gz" \
    ;
RUN cd "src/libaom" \
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
        -DENABLE_EXAMPLES=NO \
        -DENABLE_DOCS=NO \
        -DENABLE_TESTS=NO \
        -DENABLE_TOOLS=NO \
        -DCONFIG_TUNE_VMAF=1 \
        -DENABLE_NASM=ON \
        .. \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
