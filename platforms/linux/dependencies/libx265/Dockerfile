ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBX265_VERSION="38cf1c379b5af08856bb2fdd65f65a1f99384886"
ARG LIBX265_URL="https://bitbucket.org/multicoreware/x265_git/get/${LIBX265_VERSION}.tar.gz"
RUN curl -o "/tmp/libx265.tar.gz" -fsSL "$LIBX265_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libx265" \
    && tar -xzf "/tmp/libx265.tar.gz" -C "src/libx265" --strip-components 1 \
    && rm -rf "/tmp/libx265.tar.gz" \
    ;
RUN cd "src/libx265/build/linux" \
    && sed -i '/^cmake / s/$/ -DCMAKE_INSTALL_PREFIX:PATH=\/build -DENABLE_SHARED=OFF/' ./multilib.sh \
    && MAKEFLAGS="-j$(nproc)" ./multilib.sh \
    && make -C 8bit -j$(nproc) install \
    ;
