ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBFFTW_VERSION="3.3.10"
ARG LIBFFTW_URL="https://www.fftw.org/fftw-${LIBFFTW_VERSION}.tar.gz"
ARG LIBFFTW_SHA256="56c932549852cddcfafdab3820b0200c7742675be92179e59e6215b340e26467"
RUN curl -o "/tmp/libfftw.tar.gz" -fsSL "$LIBFFTW_URL" \
    && echo "$LIBFFTW_SHA256  /tmp/libfftw.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libfftw" \
    && tar -xzf "/tmp/libfftw.tar.gz" -C "src/libfftw" --strip-components 1 \
    && rm -rf "/tmp/libfftw.tar.gz" \
    ;
RUN cd "src/libfftw" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
