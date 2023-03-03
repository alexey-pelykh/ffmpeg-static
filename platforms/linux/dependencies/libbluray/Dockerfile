ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libxml2:$TAG as libxml2
FROM ${IMAGE_PREFIX}libfreetype2:$TAG as libfreetype2
FROM ${IMAGE_PREFIX}libfontconfig:$TAG as libfontconfig

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libxml2 /build .
COPY --from=libfreetype2 /build .
COPY --from=libfontconfig /build .

ARG LIBBLURAY_VERSION="1.3.4"
ARG LIBBLURAY_URL="https://code.videolan.org/videolan/libbluray/-/archive/${LIBBLURAY_VERSION}/libbluray-${LIBBLURAY_VERSION}.tar.gz"
ARG LIBBLURAY_SHA256="9820df5c3e87777be116ca225ad7ee026a3ff42b2447c7fe641910fb23aad3c2"
RUN curl -o "/tmp/libbluray.tar.gz" -fsSL "$LIBBLURAY_URL" \
    && echo "$LIBBLURAY_SHA256  /tmp/libbluray.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libbluray" \
    && tar -xzf "/tmp/libbluray.tar.gz" -C "src/libbluray" --strip-components 1 \
    && rm -rf "/tmp/libbluray.tar.gz" \
    ;

# NOTE: Instead of using 1.1.2 as would've been right, use next commit since it contains a valid fix
ARG LIBUDFREAD_VERSION="b8ada6557b5336f4d3f144d8604ddc0ccf51f0c2"
ARG LIBUDFREAD_URL="https://code.videolan.org/videolan/libudfread/-/archive/${LIBUDFREAD_VERSION}/libudfread-${LIBUDFREAD_VERSION}.tar.gz"
RUN curl -o "/tmp/libudfread.tar.gz" -fsSL "$LIBUDFREAD_URL" \
    && mkdir -p "src/libbluray/contrib/libudfread" \
    && tar -xzf "/tmp/libudfread.tar.gz" -C "src/libbluray/contrib/libudfread" --strip-components 1 \
    && rm -rf "/tmp/libbluray.tar.gz" \
    ;
RUN cd "src/libbluray" \
    && autoreconf -fiv \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
        --with-pic \
        --disable-doxygen-doc \
        --disable-doxygen-dot \
        --disable-examples \
        --disable-bdjava-jar \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
