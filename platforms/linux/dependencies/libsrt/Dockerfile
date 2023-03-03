ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libopenssl:$TAG as libopenssl

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libopenssl /build .

ARG LIBSRT_VERSION="1.5.1"
ARG LIBSRT_URL="https://github.com/Haivision/srt/archive/v${LIBSRT_VERSION}.tar.gz"
RUN curl -o "/tmp/libsrt.tar.gz" -fsSL "$LIBSRT_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libsrt" \
    && tar -xzf "/tmp/libsrt.tar.gz" -C "src/libsrt" --strip-components 1 \
    && rm -rf "/tmp/libsrt.tar.gz" \
    ;
RUN cd "src/libsrt" \
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
        -DENABLE_APPS=OFF \
        -DENABLE_LOGGING=OFF \
        -DENABLE_SHARED=OFF \
        -DENABLE_STATIC=ON \
        -DENABLE_CXX11=ON \
        -DUSE_STATIC_LIBSTDCXX=ON \
        -DOPENSSL_USE_STATIC_LIBS=ON \
        .. \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
