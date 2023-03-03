ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBOPENH264_VERSION="2.3.1"
ARG LIBOPENH264_URL="https://github.com/cisco/openh264/archive/refs/tags/v${LIBOPENH264_VERSION}.tar.gz"
RUN curl -o "/tmp/libopenh264.tar.gz" -fsSL "$LIBOPENH264_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libopenh264" \
    && tar -xzf "/tmp/libopenh264.tar.gz" -C "src/libopenh264" --strip-components 1 \
    && rm -rf "/tmp/libopenh264.tar.gz" \
    ;
RUN cd "src/libopenh264" \
    && make -j$(nproc) install-static \
        PREFIX=/build \
    && make clean \
    ;
