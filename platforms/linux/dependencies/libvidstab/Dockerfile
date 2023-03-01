ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBVIDSTAB_VERSION="90c76aca2cb06c3ff6f7476a7cd6851b39436656"
ARG LIBVIDSTAB_URL="https://github.com/georgmartius/vid.stab/archive/${LIBVIDSTAB_VERSION}.tar.gz"
RUN curl -o "/tmp/libvidstab.tar.gz" -fsSL "$LIBVIDSTAB_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libvidstab" \
    && tar -xzf "/tmp/libvidstab.tar.gz" -C "src/libvidstab" --strip-components 1 \
    && rm -rf "/tmp/libvidstab.tar.gz" \
    ;
RUN cd "src/libvidstab" \
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
