ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}builder:$TAG

ARG LIBGSM_ORIGINAL_VERSION="1.0.22"
ARG LIBGSM_ORIGINAL_URL="http://deb.debian.org/debian/pool/main/libg/libgsm/libgsm_${LIBGSM_ORIGINAL_VERSION}.orig.tar.gz"
ARG LIBGSM_PATCHES_VERSION="${LIBGSM_ORIGINAL_VERSION}-1"
ARG LIBGSM_PATCHES_URL="http://deb.debian.org/debian/pool/main/libg/libgsm/libgsm_${LIBGSM_PATCHES_VERSION}.debian.tar.xz"
RUN curl -o "/tmp/libgsm.tar.gz" -fsSL "$LIBGSM_ORIGINAL_URL" \
    && rm -rf "src" \
    && mkdir -p "src/libgsm" \
    && tar -xzf "/tmp/libgsm.tar.gz" -C "src/libgsm" --strip-components 1 \
    && rm -rf "/tmp/libgsm.tar.gz" \
    && curl -o "/tmp/libgsm-patches.tar.xz" -fsSL "$LIBGSM_PATCHES_URL" \
    && mkdir -p "src/libgsm-patches" \
    && tar -xJf "/tmp/libgsm-patches.tar.xz" -C "src/libgsm-patches" --strip-components 1 \
    && rm -rf "/tmp/libgsm-patches.tar.xz" \
    && ( \
        find "$(pwd)/src/libgsm-patches/patches" -type f -name '*.patch' -print0 \
        | sort -z \
        | xargs -t -0 -n 1 patch -p1 -d"$(pwd)/src/libgsm" -i \
    ) \
    ;
RUN cd "src/libgsm" \
    && mkdir -p "/build/include" \
    && mkdir -p "/build/lib" \
    && make -j$(nproc) gsminstall \
        INSTALL_ROOT=$(pwd) \
        GSM_INSTALL_INC=/build/include \
        GSM_INSTALL_LIB=/build/lib \
    ;
