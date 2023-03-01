ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBBZ2_VERSION="2d8393924b9f3e014000c7420c7da7c3ddb74e2c"
ARG LIBBZ2_URL="https://gitlab.com/bzip2/bzip2/-/archive/${LIBBZ2_VERSION}/bzip2-${LIBBZ2_VERSION}.tar.gz"
RUN curl -o "/tmp/libbz2.tar.gz" -fsSL "$LIBBZ2_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libbz2" \
    && tar -xzf "/tmp/libbz2.tar.gz" -C "src/libbz2" --strip-components 1 \
    && rm -rf "/tmp/libbz2.tar.gz" \
    # NOTE: https://gitlab.com/bzip2/bzip2/-/issues/46
    && sed -i 's/ARCHIVE_OUTPUT_NAME bz2_static/ARCHIVE_OUTPUT_NAME bz2/g' "src/libbz2/CMakeLists.txt" \
    ;
RUN cd "src/libbz2" \
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
        -DENABLE_DEBUG=OFF \
        -DENABLE_APP=OFF \
        -DENABLE_TESTS=OFF \
        -DENABLE_DOCS=OFF \
        -DENABLE_EXAMPLES=OFF \
        -DENABLE_STATIC_LIB=ON \
        -DENABLE_SHARED_LIB=OFF \
        .. \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
