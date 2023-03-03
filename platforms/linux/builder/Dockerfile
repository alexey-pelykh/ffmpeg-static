ARG IMAGE_PREFIX=
ARG TAG=latest

# TODO: once glibc 2.37 is available in rolling, rollback
FROM ubuntu:lunar
# FROM ubuntu:22.10

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        tzdata \
        ca-certificates \
        curl \
        build-essential \
        pkg-config \
        autoconf \
        automake \
        autopoint \
        libtool \
        gettext \
        gawk \
        bison \
        cmake \
        meson \
        gperf \
        yasm \
        nasm \
        gobject-introspection \
        gnupg \
        software-properties-common \
        libssl-dev \
        tcl \
        xxd \
        texinfo \
        git \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    ;

ENV RUSTUP_HOME=/usr/local
ENV CARGO_HOME=/usr/local
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y --no-modify-path
# NOTE: https://github.com/rust-lang/cargo/issues/10781#issuecomment-1351670409
RUN cargo --config net.git-fetch-with-cli=true install \
        cargo-c \
    ;

# Install glibc snapshot that includes https://sourceware.org/git/?p=glibc.git;a=commitdiff_plain;h=51231c469b49fb3837590f40a93f88db9a20bb86
# TODO: Remove once 2.37 is released and is available as a package
RUN add-apt-repository -y ppa:schopin/glibc-2.37-snapshot \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        libc6 \
        libc6-dev \
        libc-bin \
        libc-dev-bin \
        libc-devtools \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    ;

ENV CFLAGS="-O3 -s -static-libgcc -fno-strict-overflow -fstack-protector-all -fPIC -I/build/include"
ENV CXXFLAGS="-O3 -s -static-libgcc -fno-strict-overflow -fstack-protector-all -fPIC -I/build/include"
ENV CPPFLAGS="${CXXFLAGS}"
ENV LDFLAGS="-Wl,-z,relro,-z,now -L/build/lib"
ENV PKG_CONFIG_PATH=/build/lib/pkgconfig

WORKDIR /build
