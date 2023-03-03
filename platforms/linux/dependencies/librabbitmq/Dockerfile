ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBRABBITMQ_VERSION="0.11.0"
ARG LIBRABBITMQ_URL="https://github.com/alanxz/rabbitmq-c/archive/refs/tags/v${LIBRABBITMQ_VERSION}.tar.gz"
RUN curl -o "/tmp/librabbitmq.tar.gz" -fsSL "$LIBRABBITMQ_URL" \
    && rm -rf "src" \
    && mkdir -p "src/librabbitmq" \
    && tar -xzf "/tmp/librabbitmq.tar.gz" -C "src/librabbitmq" --strip-components 1 \
    && rm -rf "/tmp/librabbitmq.tar.gz" \
    ;
RUN cd "src/librabbitmq" \
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
        -DBUILD_EXAMPLES=OFF \
        -DBUILD_TESTS=OFF \
        -DBUILD_TOOLS=OFF \
        -DBUILD_TOOLS_DOCS=OFF \
        -DRUN_SYSTEM_TESTS=OFF \
        .. \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
