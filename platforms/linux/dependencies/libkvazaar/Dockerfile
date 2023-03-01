ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBKVAZAAR_VERSION="2.2.0"
ARG LIBKVAZAAR_URL="https://github.com/ultravideo/kvazaar/archive/v${LIBKVAZAAR_VERSION}.tar.gz"
RUN curl -o "/tmp/libkvazaar.tar.gz" -fsSL "$LIBKVAZAAR_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libkvazaar" \
    && tar -xzf "/tmp/libkvazaar.tar.gz" -C "src/libkvazaar" --strip-components 1 \
    && rm -rf "/tmp/libkvazaar.tar.gz" \
    ;
RUN cd "src/libkvazaar" \
    && ./autogen.sh \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
