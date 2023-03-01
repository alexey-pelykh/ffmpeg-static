ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBFDK_AAC_VERSION="2.0.2"
ARG LIBFDK_AAC_URL="https://github.com/mstorsjo/fdk-aac/archive/v${LIBFDK_AAC_VERSION}.tar.gz"
RUN curl -o "/tmp/libfdk-aac.tar.gz" -fsSL "$LIBFDK_AAC_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libfdk-aac" \
    && tar -xzf "/tmp/libfdk-aac.tar.gz" -C "src/libfdk-aac" --strip-components 1 \
    && rm -rf "/tmp/libfdk-aac.tar.gz" \
    ;
RUN cd "src/libfdk-aac" \
    && ./autogen.sh \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
