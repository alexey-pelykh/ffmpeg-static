ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}libaom:$TAG as libaom
FROM ${IMAGE_PREFIX}libaribb24:$TAG as libaribb24
FROM ${IMAGE_PREFIX}libass:$TAG as libass
FROM ${IMAGE_PREFIX}libavisynth:$TAG as libavisynth
FROM ${IMAGE_PREFIX}libbluray:$TAG as libbluray
FROM ${IMAGE_PREFIX}libbz2:$TAG as libbz2
FROM ${IMAGE_PREFIX}libdav1d:$TAG as libdav1d
FROM ${IMAGE_PREFIX}libdavs2:$TAG as libdavs2
FROM ${IMAGE_PREFIX}libdc1394:$TAG as libdc1394
FROM ${IMAGE_PREFIX}libfdk-aac:$TAG as libfdk-aac
FROM ${IMAGE_PREFIX}libfreetype2:$TAG as libfreetype2
FROM ${IMAGE_PREFIX}libfribidi:$TAG as libfribidi
FROM ${IMAGE_PREFIX}libgme:$TAG as libgme
FROM ${IMAGE_PREFIX}libgsm:$TAG as libgsm
FROM ${IMAGE_PREFIX}libkvazaar:$TAG as libkvazaar
FROM ${IMAGE_PREFIX}liblzma:$TAG as liblzma
FROM ${IMAGE_PREFIX}libmodplug:$TAG as libmodplug
FROM ${IMAGE_PREFIX}libmp3lame:$TAG as libmp3lame
FROM ${IMAGE_PREFIX}libmysofa:$TAG as libmysofa
FROM ${IMAGE_PREFIX}libopencore-amr:$TAG as libopencore-amr
FROM ${IMAGE_PREFIX}libopenh264:$TAG as libopenh264
FROM ${IMAGE_PREFIX}libopenjpeg:$TAG as libopenjpeg
FROM ${IMAGE_PREFIX}libopus:$TAG as libopus
FROM ${IMAGE_PREFIX}librabbitmq:$TAG as librabbitmq
FROM ${IMAGE_PREFIX}librav1e:$TAG as librav1e
FROM ${IMAGE_PREFIX}librtmp:$TAG as librtmp
FROM ${IMAGE_PREFIX}librubberband:$TAG as librubberband
FROM ${IMAGE_PREFIX}libshine:$TAG as libshine
FROM ${IMAGE_PREFIX}libsnappy:$TAG as libsnappy
FROM ${IMAGE_PREFIX}libsoxr:$TAG as libsoxr
FROM ${IMAGE_PREFIX}libspeex:$TAG as libspeex
FROM ${IMAGE_PREFIX}libsrt:$TAG as libsrt
FROM ${IMAGE_PREFIX}libssh:$TAG as libssh
FROM ${IMAGE_PREFIX}libsvtav1:$TAG as libsvtav1
FROM ${IMAGE_PREFIX}libtheora:$TAG as libtheora
FROM ${IMAGE_PREFIX}libtwolame:$TAG as libtwolame
FROM ${IMAGE_PREFIX}libuavs3d:$TAG as libuavs3d
FROM ${IMAGE_PREFIX}libvapoursynth:$TAG as libvapoursynth
FROM ${IMAGE_PREFIX}libvidstab:$TAG as libvidstab
FROM ${IMAGE_PREFIX}libvmaf:$TAG as libvmaf
FROM ${IMAGE_PREFIX}libvo-amrwbenc:$TAG as libvo-amrwbenc
FROM ${IMAGE_PREFIX}libvorbis:$TAG as libvorbis
FROM ${IMAGE_PREFIX}libvpx:$TAG as libvpx
FROM ${IMAGE_PREFIX}libwebp:$TAG as libwebp
FROM ${IMAGE_PREFIX}libx264:$TAG as libx264
FROM ${IMAGE_PREFIX}libx265:$TAG as libx265
FROM ${IMAGE_PREFIX}libxavs2:$TAG as libxavs2
FROM ${IMAGE_PREFIX}libxvidcore:$TAG as libxvidcore
FROM ${IMAGE_PREFIX}libzimg:$TAG as libzimg
FROM ${IMAGE_PREFIX}libzmq:$TAG as libzmq
FROM ${IMAGE_PREFIX}libzvbi:$TAG as libzvbi
FROM ${IMAGE_PREFIX}libopenssl:$TAG as libopenssl

FROM ${IMAGE_PREFIX}builder:$TAG

COPY --from=libaom /build .
COPY --from=libaribb24 /build .
COPY --from=libass /build .
COPY --from=libavisynth /build .
COPY --from=libbluray /build .
COPY --from=libbz2 /build .
COPY --from=libdav1d /build .
COPY --from=libdavs2 /build .
COPY --from=libdc1394 /build .
COPY --from=libfdk-aac /build .
COPY --from=libfreetype2 /build .
COPY --from=libfribidi /build .
COPY --from=libgme /build .
COPY --from=libgsm /build .
COPY --from=libkvazaar /build .
COPY --from=liblzma /build .
COPY --from=libmodplug /build .
COPY --from=libmp3lame /build .
COPY --from=libmysofa /build .
COPY --from=libopencore-amr /build .
COPY --from=libopenh264 /build .
COPY --from=libopenjpeg /build .
COPY --from=libopus /build .
COPY --from=librabbitmq /build .
COPY --from=librav1e /build .
COPY --from=librtmp /build .
COPY --from=librubberband /build .
COPY --from=libshine /build .
COPY --from=libsnappy /build .
COPY --from=libsoxr /build .
COPY --from=libspeex /build .
COPY --from=libsrt /build .
COPY --from=libssh /build .
COPY --from=libsvtav1 /build .
COPY --from=libtheora /build .
COPY --from=libtwolame /build .
COPY --from=libuavs3d /build .
COPY --from=libvapoursynth /build .
COPY --from=libvidstab /build .
COPY --from=libvmaf /build .
COPY --from=libvo-amrwbenc /build .
COPY --from=libvorbis /build .
COPY --from=libvpx /build .
COPY --from=libwebp /build .
COPY --from=libx264 /build .
COPY --from=libx265 /build .
COPY --from=libxavs2 /build .
COPY --from=libxvidcore /build .
COPY --from=libzimg /build .
COPY --from=libzmq /build .
COPY --from=libzvbi /build .
COPY --from=libopenssl /build .

# TODO: https://github.com/JuliaIO/FFMPEGBuilder/issues/4

ARG FFMPEG_VERSION="5.1.2"
ARG FFMPEG_URL="https://ffmpeg.org/releases/ffmpeg-${FFMPEG_VERSION}.tar.xz"
ARG FFMPEG_SHA256="619e706d662c8420859832ddc259cd4d4096a48a2ce1eefd052db9e440eef3dc"
RUN curl -o "/tmp/ffmpeg.tar.xz" -fsSL "$FFMPEG_URL" \
    && echo "$FFMPEG_SHA256  /tmp/ffmpeg.tar.xz" | sha256sum --status -c - \
    && rm -rf "src" \
    && mkdir -p "src/ffmpeg" \
    && tar -xJf "/tmp/ffmpeg.tar.xz" -C "src/ffmpeg" --strip-components 1 \
    && rm -rf "/tmp/ffmpeg.tar.xz" \
    ;
ARG FFMPEG_VERSION_SUFFIX=""
RUN cd "src/ffmpeg" \
    && ./configure \
        # Toolchain options:
        --prefix="/build" \
        --pkg-config-flags="--static" \
        --extra-cflags="-fopenmp -fPIE" \
        --extra-cxxflags="-fopenmp -fPIE" \
        --extra-ldflags="-fopenmp" \
        --extra-ldexeflags="-fPIE -static-pie" \
        --extra-libs="-pthread -lm" \
        --extra-version="${FFMPEG_VERSION_SUFFIX}" \
        # Developer options:
        --disable-debug \
        # Configuration options:
        --enable-static \
        --disable-shared \
        --disable-small \
        --enable-runtime-cpudetect \
        --enable-gray \
        --enable-swscale-alpha \
        # Program options:
        --enable-ffmpeg \
        --disable-ffplay \
        --enable-ffprobe \
        # Documentation options:
        --disable-doc \
        --disable-htmlpages \
        --disable-manpages \
        --disable-podpages \
        --disable-txtpages \
        # Component options:
        --enable-avdevice \
        --enable-avcodec \
        --enable-avformat \
        --enable-swresample \
        --enable-swscale \
        --enable-postproc \
        --enable-avfilter \
        --enable-pthreads \
        --disable-w32threads \
        --disable-os2threads \
        --enable-network \
        --enable-dct \
        --enable-dwt \
        --enable-error-resilience \
        --enable-lsp \
        --enable-mdct \
        --enable-rdft \
        --enable-fft \
        --enable-faan \
        --enable-pixelutils \
        # Individual component options:
        --enable-encoders \
        --enable-decoders \
        --enable-hwaccels \
        --enable-muxers \
        --enable-demuxers \
        --enable-parsers \
        --enable-bsfs \
        --enable-protocols \
        --enable-indevs \
        --enable-outdevs \
        --enable-filters \
        # Licensing options:
        --enable-gpl \
        --enable-version3 \
        --enable-nonfree \
        # External library support:
        # --disable-alsa \
        --disable-appkit \
        --disable-avfoundation \
        --enable-avisynth \
        --enable-bzlib \
        --disable-coreimage \
        # --enable-chromaprint \
        # --enable-frei0r \
        --disable-gcrypt \
        --disable-gmp \
        --disable-gnutls \
        # --disable-iconv \
        --disable-jni \
        # --enable-ladspa \
        # --enable-lcms2 \
        --enable-libaom \
        --enable-libaribb24 \
        --enable-libass \
        --enable-libbluray \
        # --enable-libbs2b \
        # --enable-libcaca \
        # --enable-libcelt \
        # --enable-libcdio \
        # --enable-libcodec2 \
        --enable-libdav1d \
        --enable-libdavs2 \
        --enable-libdc1394 \
        --enable-libfdk-aac \
        # --enable-libflite \
        --enable-libfontconfig \
        --enable-libfreetype \
        --enable-libfribidi \
        # --enable-libglslang \
        --enable-libgme \
        --enable-libgsm \
        # --enable-libiec61883 \
        # --enable-libilbc \
        # --enable-libjack \
        # --enable-libjxl \
        # --enable-libklvanc \
        --enable-libkvazaar \
        # --enable-liblensfun \
        --enable-libmodplug \
        --enable-libmp3lame \
        --enable-libmysofa \
        --enable-libopencore-amrnb \
        --enable-libopencore-amrwb \
        # --enable-libopencv \
        --enable-libopenh264 \
        --enable-libopenjpeg \
        # --enable-libopenmpt \
        # --enable-libopenvino \
        --enable-libopus \
        # --enable-libplacebo \
        # --enable-libpulse \
        --enable-librabbitmq \
        --enable-librav1e \
        # --enable-librist \
        # --enable-librsvg \
        --enable-librubberband \
        --enable-librtmp \
        # --enable-libshaderc \
        --enable-libshine \
        # -enable-libsmbclient \
        --enable-libsnappy \
        --enable-libsoxr \
        --enable-libspeex \
        --enable-libsrt \
        --enable-libssh \
        --enable-libsvtav1 \
        # --enable-libtensorflow \
        # --enable-libtesseract \
        --enable-libtheora \
        --disable-libtls \
        --enable-libtwolame \
        --enable-libuavs3d \
        # --enable-libv4l2 \
        --enable-libvidstab \
        --enable-libvmaf \
        --enable-libvo-amrwbenc \
        --enable-libvorbis \
        --enable-libvpx \
        --enable-libwebp \
        --enable-libx264 \
        --enable-libx265 \
        # --enable-libxavs \
        --enable-libxavs2 \
        --disable-libxcb \
        --disable-libxcb-shm \
        --disable-libxcb-xfixes \
        --disable-libxcb-shape \
        --enable-libxvid \
        --enable-libzimg \
        --enable-libzmq \
        --enable-libzvbi \
        # --enable-lv2 \
        --enable-lzma \
        # --enable-decklink \
        --disable-mbedtls \
        --disable-mediacodec \
        --disable-mediafoundation \
        --disable-metal \
        --enable-libmysofa \
        # --enable-openal \
        # --enable-opencl \
        # --enable-opengl \
        --enable-openssl \
        # --enable-pocketsphinx \
        # --disable-sndio \
        --disable-schannel \
        --disable-sdl2 \
        --disable-securetransport \
        --enable-vapoursynth \
        # --disable-vulkan \
        --disable-xlib \
        --enable-zlib \
    || (cat ffbuild/config.log ; false) \
    && make -j$(nproc) \
    && make install \
    && make clean \
    ;
