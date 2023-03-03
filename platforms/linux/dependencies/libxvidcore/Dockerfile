ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBXVIDCORE_VERSION="1.3.7"
ARG LIBXVIDCORE_URL="https://downloads.xvid.com/downloads/xvidcore-${LIBXVIDCORE_VERSION}.tar.gz"
ARG LIBXVIDCORE_SHA256="abbdcbd39555691dd1c9b4d08f0a031376a3b211652c0d8b3b8aa9be1303ce2d"
RUN curl -o "/tmp/libxvidcore.tar.gz" -fsSL "$LIBXVIDCORE_URL" \
    && echo "$LIBXVIDCORE_SHA256  /tmp/libxvidcore.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libxvidcore" \
    && tar -xzf "/tmp/libxvidcore.tar.gz" -C "src/libxvidcore" --strip-components 1 \
    && rm -rf "/tmp/libxvidcore.tar.gz" \
    ;
ENV CFLAGS="$CFLAGS -fstrength-reduce -ffast-math"
RUN cd "src/libxvidcore/build/generic" \
    && ./configure \
        --prefix=/build \
    && make -j$(nproc) \
    && make install \
    && rm -rf "/build/lib/libxvidcore.so"* \
    ;
