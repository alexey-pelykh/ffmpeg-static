ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libexpat:$TAG as libexpat
FROM ${IMAGE_PREFIX}libopenssl:$TAG as libopenssl
FROM ${IMAGE_PREFIX}libbz2:$TAG as libbz2
FROM ${IMAGE_PREFIX}liblzma:$TAG as liblzma
FROM ${IMAGE_PREFIX}libz:$TAG as libz

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libexpat /build .
COPY --from=libopenssl /build .
COPY --from=libbz2 /build .
COPY --from=liblzma /build .
COPY --from=libz /build .

ARG LIBPYTHON_VERSION="3.11.1"
ARG LIBPYTHON_URL="https://www.python.org/ftp/python/${LIBPYTHON_VERSION}/Python-${LIBPYTHON_VERSION}.tgz"
ARG LIBPYTHON_SHA256="baed518e26b337d4d8105679caf68c5c32630d702614fc174e98cb95c46bdfa4"
RUN curl -o "/tmp/libpython.tar.gz" -fsSL "$LIBPYTHON_URL" \
    && echo "$LIBPYTHON_SHA256  /tmp/libpython.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libpython" \
    && tar -xzf "/tmp/libpython.tar.gz" -C "src/libpython" --strip-components 1 \
    && rm -rf "/tmp/libpython.tar.gz" \
    ;
ENV PY_UNSUPPORTED_OPENSSL_BUILD=static
RUN cd "src/libpython" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
        --enable-optimizations \
        --disable-test-modules \
        --without-doc-strings \
        --with-system-expat \
        --with-openssl=/build \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
RUN /build/bin/python3 -m pip install \
    Cython \
    ;
