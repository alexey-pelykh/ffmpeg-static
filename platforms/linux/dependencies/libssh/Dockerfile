ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libopenssl:$TAG as libopenssl
FROM ${IMAGE_PREFIX}libz:$TAG as libz

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libopenssl /build .
COPY --from=libz /build .

ARG LIBSSH_VERSION="0.10.4"
ARG LIBSSH_URL="https://git.libssh.org/projects/libssh.git/snapshot/libssh-${LIBSSH_VERSION}.tar.xz"
RUN curl -o "/tmp/libssh.tar.xz" -fsSL "$LIBSSH_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libssh" \
    && tar -xJf "/tmp/libssh.tar.xz" -C "src/libssh" --strip-components 1 \
    && rm -rf "/tmp/libssh.tar.xz" \
    ;
RUN cd "src/libssh" \
    && mkdir ".cmake" \
    && cd ".cmake" \
    && cmake \
        -G"Unix Makefiles" \
        -DCMAKE_INSTALL_PREFIX:PATH=/build \
        -DBUILD_SHARED_LIBS=OFF \
        -DBUILD_STATIC_LIBS=ON \
        -DCMAKE_INSTALL_LIBDIR=lib \
        -DCMAKE_VERBOSE_MAKEFILE=ON \
        -DCMAKE_BUILD_TYPE=Release \
        -DWITH_ZLIB=ON \
        -DWITH_GCRYPT=OFF \
        -DWITH_MBEDTLS=OFF \
        -DWITH_NACL=OFF \
        -DWITH_SFTP=ON \
        -DWITH_SERVER=OFF \
        -DWITH_GSSAPI=OFF \
        -DWITH_GEX=ON \
        -DWITH_EXAMPLES=OFF \
        -DUNIT_TESTING=OFF \
        -DBUILD_STATIC_LIB=ON \
        -DBUILD_SHARED_LIBS=OFF \
        -DWITH_BLOWFISH_CIPHER=ON \
        -DWITH_PCAP=ON -DWITH_DEBUG_CRYPTO=OFF -DWITH_DEBUG_PACKET=OFF -DWITH_DEBUG_CALLTRACE=OFF -DWITH_DSA=ON \
        .. \
    && echo "Requires.private: libssl libcrypto zlib" >> ../libssh.pc.cmake \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
