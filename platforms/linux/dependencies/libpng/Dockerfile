ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libz:$TAG as libz

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libz /build .

ARG LIBPNG_VERSION="1.6.39"
ARG LIBPNG_URL="https://download.sourceforge.net/libpng/libpng-${LIBPNG_VERSION}.tar.gz"
ARG LIBPNG_SHA256="af4fb7f260f839919e5958e5ab01a275d4fe436d45442a36ee62f73e5beb75ba"
RUN curl -o "/tmp/libpng.tar.gz" -fsSL "$LIBPNG_URL" \
    && echo "$LIBPNG_SHA256  /tmp/libpng.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libpng" \
    && tar -xzf "/tmp/libpng.tar.gz" -C "src/libpng" --strip-components 1 \
    && rm -rf "/tmp/libpng.tar.gz" \
    ;
RUN cd "src/libpng" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
