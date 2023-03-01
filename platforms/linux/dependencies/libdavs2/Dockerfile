ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBDAVS2_VERSION="1.7"
ARG LIBDAVS2_URL="https://github.com/pkuvcl/davs2/archive/refs/tags/${LIBDAVS2_VERSION}.tar.gz"
RUN curl -o "/tmp/libdavs2.tar.gz" -fsSL "$LIBDAVS2_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libdavs2" \
    && tar -xzf "/tmp/libdavs2.tar.gz" -C "src/libdavs2" --strip-components 1 \
    && rm -rf "/tmp/libdavs2.tar.gz" \
    ;
RUN cd "src/libdavs2/build/linux" \
    && ./configure \
        --prefix=/build \
        --enable-pic \
        --disable-asm \
        --disable-cli \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
