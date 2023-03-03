ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBZIMG_VERSION="3.0.4"
ARG LIBZIMG_URL="https://github.com/sekrit-twc/zimg/archive/release-${LIBZIMG_VERSION}.tar.gz"
RUN curl -o "/tmp/libzimg.tar.gz" -fsSL "$LIBZIMG_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libzimg" \
    && tar -xzf "/tmp/libzimg.tar.gz" -C "src/libzimg" --strip-components 1 \
    && rm -rf "/tmp/libzimg.tar.gz" \
    ;
RUN cd "src/libzimg" \
    && ./autogen.sh \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
