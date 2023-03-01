ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libopenssl:$TAG as libopenssl
FROM ${IMAGE_PREFIX}libz:$TAG as libz

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libopenssl /build .
COPY --from=libz /build .

# NOTE: Following https://github.com/freebsd/freebsd-ports/blob/main/multimedia/librtmp/Makefile
ARG LIBRTMP_VERSION="f1b83c10d8beb43fcc70a6e88cf4325499f25857"
ARG LIBRTMP_URL="https://git.ffmpeg.org/gitweb/rtmpdump.git/snapshot/${LIBRTMP_VERSION}.tar.gz?/"
RUN curl -o "/tmp/librtmp.tar.gz" -fsSL "$LIBRTMP_URL" \
    && rm -rf "src" \
    && mkdir -p "src/librtmp" \
    && tar -xzf "/tmp/librtmp.tar.gz" -C "src/librtmp" --strip-components 1 \
    && rm -rf "/tmp/librtmp.tar.gz" \
    && for LIBRTMP_PATCHED_FILE in dh.h handshake.h hashswf.c; \
    do \
        curl -fsSL "https://raw.githubusercontent.com/freebsd/freebsd-ports/main/multimedia/librtmp/files/patch-librtmp-${LIBRTMP_PATCHED_FILE}" | patch -p1 "src/librtmp/librtmp/${LIBRTMP_PATCHED_FILE}"; \
    done \
    ;
RUN cd "src/librtmp" \
    && make -j$(nproc) install \
        prefix=/build \
        SHARED=no \
        XCFLAGS=-I/build/include \
        XLDFLAGS=-L/build/lib \
    ;
