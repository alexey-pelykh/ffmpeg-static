ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBMODPLUG_VERSION="0.8.9.0"
ARG LIBMODPLUG_URL="https://downloads.sourceforge.net/modplug-xmms/libmodplug-${LIBMODPLUG_VERSION}.tar.gz"
ARG LIBMODPLUG_SHA256="457ca5a6c179656d66c01505c0d95fafaead4329b9dbaa0f997d00a3508ad9de"
RUN curl -o "/tmp/libmodplug.tar.gz" -fsSL "$LIBMODPLUG_URL" \
    && echo "$LIBMODPLUG_SHA256  /tmp/libmodplug.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libmodplug" \
    && tar -xzf "/tmp/libmodplug.tar.gz" -C "src/libmodplug" --strip-components 1 \
    && rm -rf "/tmp/libmodplug.tar.gz" \
    ;
RUN cd "src/libmodplug" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
