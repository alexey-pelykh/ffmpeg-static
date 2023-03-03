ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBUAVS3D_VERSION="0133ee4b4bbbef7b88802e7ad019b14b9b852c2b"
ARG LIBUAVS3D_URL="https://github.com/uavs3/uavs3d/archive/${LIBUAVS3D_VERSION}.tar.gz"
RUN curl -o "/tmp/libuavs3d.tar.gz" -fsSL "$LIBUAVS3D_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libuavs3d" \
    && tar -xzf "/tmp/libuavs3d.tar.gz" -C "src/libuavs3d" --strip-components 1 \
    && rm -rf "/tmp/libuavs3d.tar.gz" \
    ;
RUN cd "src/libuavs3d" \
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
