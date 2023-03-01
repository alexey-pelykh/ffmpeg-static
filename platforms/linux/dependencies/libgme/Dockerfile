ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBGME_VERSION="6cd4bdb69be304f58c9253fb08b8362f541b3b4b"
ARG LIBGME_URL="https://bitbucket.org/mpyne/game-music-emu/get/${LIBGME_VERSION}.tar.gz"
RUN curl -o "/tmp/libgme.tar.gz" -fsSL "$LIBGME_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libgme" \
    && tar -xzf "/tmp/libgme.tar.gz" -C "src/libgme" --strip-components 1 \
    && rm -rf "/tmp/libgme.tar.gz" \
    ;
RUN cd "src/libgme" \
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
        -DENABLE_UBSAN=OFF \
        .. \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
