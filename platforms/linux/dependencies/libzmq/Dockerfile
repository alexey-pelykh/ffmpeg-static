ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBZMQ_VERSION="4.3.4"
ARG LIBZMQ_URL="https://github.com/zeromq/libzmq/releases/download/v${LIBZMQ_VERSION}/zeromq-${LIBZMQ_VERSION}.tar.gz"
RUN curl -o "/tmp/libzmq.tar.gz" -fsSL "$LIBZMQ_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libzmq" \
    && tar -xzf "/tmp/libzmq.tar.gz" -C "src/libzmq" --strip-components 1 \
    && rm -rf "/tmp/libzmq.tar.gz" \
    # NOTE: https://github.com/zeromq/libzmq/issues/4206
    # NOTE: https://github.com/zeromq/libzmq/issues/4374
    && (curl -fsSL "https://github.com/zeromq/libzmq/commit/92b2c38a2c51a1942a380c7ee08147f7b1ca6845.diff" | patch -p1 -d"src/libzmq") \
    && (curl -fsSL "https://github.com/zeromq/libzmq/commit/176d72cc9b3bdcc416fd11dbc82e7b386dda32b7.diff" | patch -p1 -d"src/libzmq") \
    ;
RUN cd "src/libzmq" \
    && ./configure \
        --prefix=/build \
        --disable-shared \
        --enable-static \
        --disable-ws \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
