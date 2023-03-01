ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libfftw:$TAG as libfftw
FROM ${IMAGE_PREFIX}libsamplerate:$TAG as libsamplerate

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libfftw /build .
COPY --from=libsamplerate /build .

ARG LIBRUBBERBAND_VERSION="2.0.2"
ARG LIBRUBBERBAND_URL="https://breakfastquay.com/files/releases/rubberband-${LIBRUBBERBAND_VERSION}.tar.bz2"
ARG LIBRUBBERBAND_SHA256="b9eac027e797789ae99611c9eaeaf1c3a44cc804f9c8a0441a0d1d26f3d6bdf9"
RUN curl -o "/tmp/librubberband.tar.bz2" -fsSL "$LIBRUBBERBAND_URL" \
    && echo "$LIBRUBBERBAND_SHA256  /tmp/librubberband.tar.bz2" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/librubberband" \
    && tar -xjf "/tmp/librubberband.tar.bz2" -C "src/librubberband" --strip-components 1 \
    && rm -rf "/tmp/librubberband.tar.bz2" \
    ;
RUN cd "src/librubberband" \
    && meson setup \
        --prefix=/build \
        --libdir=lib \
        --default-library=static \
        --buildtype=release \
        ".meson" \
        -Dfft=fftw \
        -Dresampler=libsamplerate \
    && ninja -j$(nproc) -v -C ".meson" install \
    && ninja -j$(nproc) -v -C ".meson" clean \
    && echo "Requires.private: fftw3 samplerate" >> /build/lib/pkgconfig/rubberband.pc \
    ;
