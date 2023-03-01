ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBVO_AMRWBENC_VERSION="0.1.3"
ARG LIBVO_AMRWBENC_URL="https://downloads.sourceforge.net/project/opencore-amr/vo-amrwbenc/vo-amrwbenc-${LIBVO_AMRWBENC_VERSION}.tar.gz"
ARG LIBVO_AMRWBENC_SHA256="5652b391e0f0e296417b841b02987d3fd33e6c0af342c69542cbb016a71d9d4e"
RUN curl -o "/tmp/libvo-amrwbenc.tar.gz" -fsSL "$LIBVO_AMRWBENC_URL" \
    && echo "$LIBVO_AMRWBENC_SHA256  /tmp/libvo-amrwbenc.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libvo-amrwbenc" \
    && tar -xzf "/tmp/libvo-amrwbenc.tar.gz" -C "src/libvo-amrwbenc" --strip-components 1 \
    && rm -rf "/tmp/libvo-amrwbenc.tar.gz" \
    ;
RUN cd "src/libvo-amrwbenc" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
