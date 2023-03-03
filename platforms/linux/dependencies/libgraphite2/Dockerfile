ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBGRAPHITE2_ORIGINAL_VERSION="1.3.14"
ARG LIBGRAPHITE2_ORIGINAL_URL="http://deb.debian.org/debian/pool/main/g/graphite2/graphite2_${LIBGRAPHITE2_ORIGINAL_VERSION}.orig.tar.gz"
ARG LIBGRAPHITE2_PATCHES_VERSION="${LIBGRAPHITE2_ORIGINAL_VERSION}-1"
ARG LIBGRAPHITE2_PATCHES_URL="http://deb.debian.org/debian/pool/main/g/graphite2/graphite2_${LIBGRAPHITE2_PATCHES_VERSION}.debian.tar.xz"
RUN curl -o "/tmp/libgraphite2.tar.gz" -fsSL "$LIBGRAPHITE2_ORIGINAL_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libgraphite2" \
    && tar -xzf "/tmp/libgraphite2.tar.gz" -C "src/libgraphite2" --strip-components 1 \
    && rm -rf "/tmp/libgraphite2.tar.gz" \
    && curl -o "/tmp/libgraphite2-patches.tar.xz" -fsSL "$LIBGRAPHITE2_PATCHES_URL" \
    && mkdir -p "src/libgraphite2-patches" \
    && tar -xJf "/tmp/libgraphite2-patches.tar.xz" -C "src/libgraphite2-patches" --strip-components 1 \
    && rm -rf "/tmp/libgraphite2-patches.tar.xz" \
    && ( \
        while read PATCH_FILE; do \
            patch -p1 -d"$(pwd)/src/libgraphite2" -i "$(pwd)/src/libgraphite2-patches/patches/${PATCH_FILE}"; \
        done <"src/libgraphite2-patches/patches/series" \
    ) \
    ;
RUN cd "src/libgraphite2" \
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
