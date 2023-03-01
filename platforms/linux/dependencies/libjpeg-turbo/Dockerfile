ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBJPEG_TURBO_VERSION="2.1.5.1"
ARG LIBJPEG_TURBO_URL="https://downloads.sourceforge.net/project/libjpeg-turbo/${LIBJPEG_TURBO_VERSION}/libjpeg-turbo-${LIBJPEG_TURBO_VERSION}.tar.gz"
ARG LIBJPEG_TURBO_SHA256="2fdc3feb6e9deb17adec9bafa3321419aa19f8f4e5dea7bf8486844ca22207bf"
RUN curl -o "/tmp/libjpeg-turbo.tar.gz" -fsSL "$LIBJPEG_TURBO_URL" \
    && echo "$LIBJPEG_TURBO_SHA256  /tmp/libjpeg-turbo.tar.gz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/libjpeg-turbo" \
    && tar -xzf "/tmp/libjpeg-turbo.tar.gz" -C "src/libjpeg-turbo" --strip-components 1 \
    && rm -rf "/tmp/libjpeg-turbo.tar.gz" \
    ;
RUN cd "src/libjpeg-turbo" \
    && mkdir ".cmake" \
    && cd ".cmake" \
    && cmake \
        -G"Unix Makefiles" \
        -DCMAKE_INSTALL_PREFIX:PATH=/build \
        -DBUILD_SHARED_LIBS=OFF \
        -DBUILD_STATIC_LIBS=ON \
        -DCMAKE_VERBOSE_MAKEFILE=ON \
        -DCMAKE_BUILD_TYPE=Release \
        -DENABLE_SHARED=OFF \
        -DENABLE_STATIC=ON \
        .. \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
