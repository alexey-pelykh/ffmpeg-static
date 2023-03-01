ARG IMAGE_PREFIX=
ARG TAG=latest

FROM ${IMAGE_PREFIX}ffmpeg:$TAG as ffmpeg

FROM scratch

WORKDIR /

ENV PATH=/

COPY --from=ffmpeg /build/bin/ffmpeg .
COPY --from=ffmpeg /build/bin/ffprobe .
