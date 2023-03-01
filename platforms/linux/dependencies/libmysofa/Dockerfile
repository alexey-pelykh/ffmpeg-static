ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libz:$TAG as libz

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libz /build .

ARG LIBMYSOFA_VERSION="1.3.1"
ARG LIBMYSOFA_URL="https://github.com/hoene/libmysofa/archive/refs/tags/v${LIBMYSOFA_VERSION}.tar.gz"
RUN curl -o "/tmp/libmysofa.tar.gz" -fsSL "$LIBMYSOFA_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libmysofa" \
    && tar -xzf "/tmp/libmysofa.tar.gz" -C "src/libmysofa" --strip-components 1 \
    && rm -rf "/tmp/libmysofa.tar.gz" \
    ;
RUN cd "src/libmysofa" \
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
        -DBUILD_TESTS=OFF \
        .. \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
