ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBOPENJPEG_VERSION="2.5.0"
ARG LIBOPENJPEG_URL="https://github.com/uclouvain/openjpeg/archive/v${LIBOPENJPEG_VERSION}.tar.gz"
RUN curl -o "/tmp/libopenjpeg.tar.gz" -fsSL "$LIBOPENJPEG_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libopenjpeg" \
    && tar -xzf "/tmp/libopenjpeg.tar.gz" -C "src/libopenjpeg" --strip-components 1 \
    && rm -rf "/tmp/libopenjpeg.tar.gz" \
    ;
RUN cd "src/libopenjpeg" \
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
        -DBUILD_PKGCONFIG_FILES=ON \
        -DBUILD_CODEC=OFF \
        -DWITH_ASTYLE=OFF \
        -DBUILD_TESTING=OFF \
        .. \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
