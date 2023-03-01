ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBRAV1E_VERSION="0.6.3"
ARG LIBRAV1E_URL="https://github.com/xiph/rav1e/archive/v${LIBRAV1E_VERSION}.tar.gz"
RUN curl -o "/tmp/librav1e.tar.gz" -fsSL "$LIBRAV1E_URL" \
    && rm -rf "src" \
    && mkdir -p "src/librav1e" \
    && tar -xzf "/tmp/librav1e.tar.gz" -C "src/librav1e" --strip-components 1 \
    && rm -rf "/tmp/librav1e.tar.gz" \
    ;
# NOTE: The following is needed to remove dependency on libgcc_s
ENV RUSTFLAGS="${RUSTFLAGS} -C target-feature=+crt-static"
RUN cd "src/librav1e" \
    && cargo cinstall \
        --prefix=/build \
        --crt-static \
        --release \
        --lib \
        --library-type staticlib \
    && cargo clean \
    ;
