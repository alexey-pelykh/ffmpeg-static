ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libfreetype2:$TAG as libfreetype2
FROM ${IMAGE_PREFIX}libfribidi:$TAG as libfribidi
FROM ${IMAGE_PREFIX}libharfbuzz:$TAG as libharfbuzz
FROM ${IMAGE_PREFIX}libfontconfig:$TAG as libfontconfig

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libfreetype2 /build .
COPY --from=libfribidi /build .
COPY --from=libharfbuzz /build .
COPY --from=libfontconfig /build .

ARG LIBASS_VERSION="0.17.0"
ARG LIBASS_URL="https://github.com/libass/libass/releases/download/${LIBASS_VERSION}/libass-${LIBASS_VERSION}.tar.gz"
ARG LIBASS_SHA256="72b9ba5d9dd1ac6d30b5962f38cbe7aefb180174f71d8b65c5e3c3060dbc403f"
RUN curl -o "/tmp/libass.tar.gz" -fsSL "$LIBASS_URL" \
    && echo "$LIBASS_SHA256  /tmp/libass.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libass" \
    && tar -xzf "/tmp/libass.tar.gz" -C "src/libass" --strip-components 1 \
    && rm -rf "/tmp/libass.tar.gz" \
    ;
RUN cd "src/libass" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
