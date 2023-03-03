ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libgraphite2:$TAG as libgraphite2

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libgraphite2 /build .

ARG LIBHARFBUZZ_VERSION="6.0.0"
ARG LIBHARFBUZZ_URL="https://github.com/harfbuzz/harfbuzz/releases/download/${LIBHARFBUZZ_VERSION}/harfbuzz-${LIBHARFBUZZ_VERSION}.tar.xz"
ARG LIBHARFBUZZ_SHA256="1d1010a1751d076d5291e433c138502a794d679a7498d1268ee21e2d4a140eb4"
RUN curl -o "/tmp/libharfbuzz.tar.xz" -fsSL "$LIBHARFBUZZ_URL" \
    && echo "$LIBHARFBUZZ_SHA256  /tmp/libharfbuzz.tar.xz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libharfbuzz" \
    && tar -xJf "/tmp/libharfbuzz.tar.xz" -C "src/libharfbuzz" --strip-components 1 \
    && rm -rf "/tmp/libharfbuzz.tar.xz" \
    ;
RUN cd "src/libharfbuzz" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
