ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libfreetype2:$TAG as libfreetype2
FROM ${IMAGE_PREFIX}libexpat:$TAG as libexpat

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libfreetype2 /build .
COPY --from=libexpat /build .

ARG LIBFONTCONFIG_VERSION="2.14.2"
ARG LIBFONTCONFIG_URL="https://www.freedesktop.org/software/fontconfig/release/fontconfig-${LIBFONTCONFIG_VERSION}.tar.gz"
ARG LIBFONTCONFIG_SHA256="3ba2dd92158718acec5caaf1a716043b5aa055c27b081d914af3ccb40dce8a55"
RUN curl -o "/tmp/libfontconfig.tar.gz" -fsSL "$LIBFONTCONFIG_URL" \
    && echo "$LIBFONTCONFIG_SHA256  /tmp/libfontconfig.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libfontconfig" \
    && tar -xzf "/tmp/libfontconfig.tar.gz" -C "src/libfontconfig" --strip-components 1 \
    && rm -rf "/tmp/libfontconfig.tar.gz" \
    ;
RUN cd "src/libfontconfig" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
