ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libz:$TAG as libz

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libz /build .

ARG LIBFREETYPE2_VERSION="2.12.1"
ARG LIBFREETYPE2_URL="https://download.savannah.gnu.org/releases/freetype/freetype-${LIBFREETYPE2_VERSION}.tar.gz"
ARG LIBFREETYPE2_SHA256="efe71fd4b8246f1b0b1b9bfca13cfff1c9ad85930340c27df469733bbb620938"
RUN curl -o "/tmp/libfreetype2.tar.gz" -fsSL "$LIBFREETYPE2_URL" \
    && echo "$LIBFREETYPE2_SHA256  /tmp/libfreetype2.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libfreetype2" \
    && tar -xzf "/tmp/libfreetype2.tar.gz" -C "src/libfreetype2" --strip-components 1 \
    && rm -rf "/tmp/libfreetype2.tar.gz" \
    ;
RUN cd "src/libfreetype2" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
