FROM        alpine:latest AS base

RUN         apk add --no-cache --update libgcc libstdc++ ca-certificates libcrypto1.1 libssl1.1 libgomp expat git

FROM        base AS build

WORKDIR     /tmp/workdir

ENV SRC=/usr/local

ARG         LD_LIBRARY_PATH=/opt/ffmpeg/lib
ARG         MAKEFLAGS="-j2"
ARG         PKG_CONFIG_PATH="/opt/ffmpeg/share/pkgconfig:/opt/ffmpeg/lib/pkgconfig:/opt/ffmpeg/lib64/pkgconfig"
ARG         PREFIX=/opt/ffmpeg
ARG         LD_LIBRARY_PATH="/opt/ffmpeg/lib:/opt/ffmpeg/lib64:/usr/lib64:/usr/lib:/lib64:/lib"

RUN ["mkdir", "-p", "/tmp/sources/"]

COPY ./sources/* /tmp/sources/

RUN buildDeps="autoconf \
  libpng-dev \
  jpeg-dev \
  giflib-dev \
  tiff-dev \
  automake \
  bash \
  binutils \
  bzip2 \
  cmake \
  curl \
  coreutils \
  diffutils \
  file \
  g++ \
  gcc \
  gperf \
  libtool \
  make \
  python3 \
  openssl-dev \
  tar \
  yasm \
  nasm \
  zlib-dev \
  expat-dev" && \
  apk add --no-cache --update ${buildDeps}
  
### libopus https://www.opus-codec.org/
RUN \
  DIR=/tmp/opus && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/opus-1.2.tar.gz | tar -zx --strip-components=1 && \
  autoreconf -fiv && \
  ./configure --prefix="${PREFIX}" --enable-shared && \
  make && \
  make install && \
  rm -rf ${DIR}
### libogg https://www.xiph.org/ogg/
RUN \
  DIR=/tmp/ogg && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/libogg-1.3.2.tar.gz | tar -xz --strip-components=1 && \
  ./configure --prefix="${PREFIX}" --enable-shared  && \
  make && \
  make install && \
  rm -rf ${DIR}
### libtheora http://www.theora.org/
RUN \
  DIR=/tmp/theora && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/libtheora-1.1.1.tar.gz | tar -xz --strip-components=1 && \
  ./configure --prefix="${PREFIX}" --with-ogg="${PREFIX}" --enable-shared && \
  make && \
  make install && \
  rm -rf ${DIR}
## opencore-amr https://sourceforge.net/projects/opencore-amr/
RUN \
  DIR=/tmp/opencore-amr && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/opencore-amr-0.1.5.tar.gz | tar -xz --strip-components=1 && \
  ./configure --prefix="${PREFIX}" --enable-shared  && \
  make && \
  make install && \
  rm -rf ${DIR}
### libvorbis https://xiph.org/vorbis/
RUN \
  DIR=/tmp/vorbis && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/libvorbis-1.3.5.tar.xz | tar -xJ --strip-components=1 && \
  ./configure --prefix="${PREFIX}" --with-ogg="${PREFIX}" --enable-shared && \
  make && \
  make install && \
  rm -rf ${DIR}
### libvpx https://www.webmproject.org/code/
RUN \
  DIR=/tmp/vpx && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/libvpx-1.8.0.tar.gz | tar -zx --strip-components=1 && \
  ./configure --prefix="${PREFIX}" --enable-vp8 --enable-vp9 --enable-vp9-highbitdepth --enable-pic --enable-shared \
  --disable-debug --disable-examples --disable-docs --disable-install-bins  && \
  make && \
  make install && \
  rm -rf ${DIR}
### libmp3lame http://lame.sourceforge.net/
RUN \
  DIR=/tmp/lame && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/lame-3.100.tar.gz | tar -zx --strip-components=1 && \
  ./configure --prefix="${PREFIX}" --bindir="${PREFIX}/bin" --enable-shared --enable-nasm --disable-frontend && \
  make && \
  make install && \
  rm -rf ${DIR}
### xvid https://www.xvid.com/
RUN \
  DIR=/tmp/xvid && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/xvidcore-1.3.4.tar.gz | tar -zx --strip-components=1 && \
  cd ./build/generic && \
  ./configure --prefix="${PREFIX}" --bindir="${PREFIX}/bin" && \
  make && \
  make install && \
  rm -rf ${DIR}
### fdk-aac https://github.com/mstorsjo/fdk-aac
RUN \
  DIR=/tmp/fdk-aac && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/fdk-aac-0.1.5.tar.gz | tar -zx --strip-components=1 && \
  autoreconf -fiv && \
  ./configure --prefix="${PREFIX}" --enable-shared --datadir="${DIR}" && \
  make && \
  make install && \
  rm -rf ${DIR}
## openjpeg https://github.com/uclouvain/openjpeg
RUN \
  DIR=/tmp/openjpeg && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/openjpeg-2.1.2.tar.gz | tar -zx --strip-components=1 && \
  cmake -DBUILD_THIRDPARTY:BOOL=ON -DCMAKE_INSTALL_PREFIX="${PREFIX}" . && \
  make && \
  make install && \
  rm -rf ${DIR}
## freetype https://www.freetype.org/
RUN  \
  DIR=/tmp/freetype && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/freetype-2.5.5.tar.gz | tar -zx --strip-components=1 && \
  ./configure --prefix="${PREFIX}" --disable-static --enable-shared && \
  make && \
  make install && \
  rm -rf ${DIR}
## libvstab https://github.com/georgmartius/vid.stab
RUN  \
  DIR=/tmp/vid.stab && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/libvidstab-1.1.0.tar.gz | tar -zx --strip-components=1 && \
  cmake -DCMAKE_INSTALL_PREFIX="${PREFIX}" . && \
  make && \
  make install && \
  rm -rf ${DIR}
## fridibi https://www.fribidi.org/
RUN  \
  DIR=/tmp/fribidi && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/fribidi-0.19.7.tar.gz | tar -zx --strip-components=1 && \
  sed -i 's/^SUBDIRS =.*/SUBDIRS=gen.tab charset lib bin/' Makefile.am && \
  ./bootstrap --no-config --auto && \
  ./configure --prefix="${PREFIX}" --disable-static --enable-shared && \
  make -j1 && \
  make install && \
  rm -rf ${DIR}
## fontconfig https://www.freedesktop.org/wiki/Software/fontconfig/
RUN  \
  DIR=/tmp/fontconfig && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/fontconfig-2.12.4.tar.bz2 | tar -jx --strip-components=1 && \
  ./configure --prefix="${PREFIX}" --disable-static --enable-shared && \
  make && \
  make install && \
  rm -rf ${DIR}
## libass https://github.com/libass/libass
RUN  \
  DIR=/tmp/libass && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/libass-0.13.7.tar.gz | tar -zx --strip-components=1 && \
  ./autogen.sh && \
  ./configure --prefix="${PREFIX}" --disable-static --enable-shared && \
  make && \
  make install && \
  rm -rf ${DIR}
## kvazaar https://github.com/ultravideo/kvazaar
RUN \
  DIR=/tmp/kvazaar && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/kvazaar-1.2.0.tar.gz | tar -zx --strip-components=1 && \
  ./autogen.sh && \
  ./configure --prefix="${PREFIX}" --disable-static --enable-shared && \
  make && \
  make install && \
  rm -rf ${DIR}

RUN \
  DIR=/tmp/aom && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/aom-1.0.0.tar.gz | tar -zx --strip-components=1 && \
  rm -rf CMakeCache.txt CMakeFiles && \
  mkdir -p ./aom_build && \
  cd ./aom_build && \
  cmake -DCMAKE_INSTALL_PREFIX="${PREFIX}" -DBUILD_SHARED_LIBS=1 .. && \
  make && \
  make install && \
  rm -rf ${DIR}

RUN \
  DIR=/tmp/xorg-macros && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/util-macros-1.19.2.tar.gz  | tar -zx --strip-components=1 && \
  ./configure --srcdir=${DIR} --prefix="${PREFIX}" && \
  make && \
  make install && \
  rm -rf ${DIR}

RUN \
  DIR=/tmp/xproto && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/xproto-7.0.31.tar.gz | tar -zx --strip-components=1 && \
  ./configure --srcdir=${DIR} --prefix="${PREFIX}" && \
  make && \
  make install && \
  rm -rf ${DIR}

RUN \
  DIR=/tmp/libXau && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/libXau-1.0.9.tar.gz | tar -zx --strip-components=1 && \
  ./configure --srcdir=${DIR} --prefix="${PREFIX}" && \
  make && \
  make install && \
  rm -rf ${DIR}

RUN \
  DIR=/tmp/libpthread-stubs && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/libpthread-stubs-0.4.tar.gz | tar -zx --strip-components=1 && \
  ./configure --prefix="${PREFIX}" && \
  make && \
  make install && \
  rm -rf ${DIR}

RUN \
  DIR=/tmp/libxcb-proto && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/xcb-proto-1.13.tar.gz | tar -zx --strip-components=1 && \
  ACLOCAL_PATH="${PREFIX}/share/aclocal" ./autogen.sh && \
  ./configure --prefix="${PREFIX}" && \
  make && \
  make install && \
  rm -rf ${DIR}

## libxcb (and supporting libraries) for screen capture https://xcb.freedesktop.org/
RUN \
  DIR=/tmp/libxcb && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/libxcb-1.13.1.tar.gz | tar -zx --strip-components=1 && \
  ACLOCAL_PATH="${PREFIX}/share/aclocal" ./autogen.sh && \
  ./configure --prefix="${PREFIX}" --disable-static --enable-shared && \
  make && \
  make install && \
  rm -rf ${DIR}

## libxml2 - for libbluray
RUN \
  DIR=/tmp/libxml2 && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/libxml2-v2.9.10.tar.gz | tar -xz --strip-components=1 && \
  ./autogen.sh --prefix="${PREFIX}" --with-ftp=no --with-http=no --with-python=no && \
  make && \
  make install && \
  rm -rf ${DIR}

## libbluray - Requires libxml, freetype, and fontconfig
RUN \
  DIR=/tmp/libbluray && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/libbluray-1.1.2.tar.bz2 | tar -jx --strip-components=1 && \
  ./configure --prefix="${PREFIX}" --disable-examples --disable-bdjava-jar --disable-static --enable-shared && \
  make && \
  make install && \
  rm -rf ${DIR}

## libzmq https://github.com/zeromq/libzmq/
RUN \
  DIR=/tmp/libzmq && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/libzmq-4.3.2.tar.gz | tar -xz --strip-components=1 && \
  ./autogen.sh && \
  ./configure --prefix="${PREFIX}" && \
  make && \
  # make check && \
  make install && \
  rm -rf ${DIR}
  
### libwebp https://developers.google.com/speed/webp/
RUN \
  DIR=/tmp/vebp && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/libwebp-1.0.2.tar.gz | tar -zx --strip-components=1 && \
  ./configure --prefix="${PREFIX}" --enable-shared  && \
  make && \
  make install && \
  rm -rf ${DIR}

## x264 http://www.videolan.org/developers/x264.html
RUN \
  DIR=/tmp/x264 && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/x264-snapshot-20170226-2245-stable.tar.bz2 | tar -jx --strip-components=1 && \
  ./configure --prefix="${PREFIX}" --enable-shared --enable-pic --disable-cli && \
  make && \
  make install && \
  rm -rf ${DIR}
### x265 http://x265.org/
RUN \
  DIR=/tmp/x265 && \
  mkdir -p ${DIR} && \
  cd ${DIR} && \
  cat /tmp/sources/x265_3.1.1.tar.gz | tar -zx --strip-components=1 && \
  cd build/linux && \
  sed -i "/-DEXTRA_LIB/ s/$/ -DCMAKE_INSTALL_PREFIX=\${PREFIX}/" multilib.sh && \
  sed -i "/^cmake/ s/$/ -DENABLE_CLI=OFF/" multilib.sh && \
  ./multilib.sh && \
  make -C 8bit install && \
  rm -rf ${DIR}  
  
## ffmpeg https://ffmpeg.org/
RUN  \
  DIR=/tmp/ffmpeg && mkdir -p ${DIR} && cd ${DIR} && \
  cat /tmp/sources/ffmpeg-4.2.2.tar.bz2 | tar -jx --strip-components=1


RUN \
  DIR=/tmp/ffmpeg && mkdir -p ${DIR} && cd ${DIR} && \
  ./configure \
  --disable-debug \
  --disable-doc \
  --disable-ffplay \
  --enable-shared \
  --enable-avresample \
  --enable-libopencore-amrnb \
  --enable-libopencore-amrwb \
  --enable-gpl \
  --enable-libass \
  --enable-fontconfig \
  --enable-libfreetype \
  --enable-libvidstab \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libtheora \
  --enable-libvorbis \
  --enable-libvpx \
  --enable-libwebp \
  --enable-libxcb \ 
  --enable-libx265 \
  --enable-libxvid \
  --enable-libx264 \
  --enable-nonfree \
  --enable-openssl \
  --enable-libfdk_aac \
  --enable-postproc \
  --enable-small \
  --enable-version3 \
  --enable-libbluray \
  --enable-libzmq \
  --extra-libs=-ldl \
  --prefix="${PREFIX}" \
  --enable-libopenjpeg \
  --enable-libkvazaar \
  --enable-libaom \
  --extra-libs=-lpthread \
  --extra-cflags="-I${PREFIX}/include" \
  --extra-ldflags="-L${PREFIX}/lib" && \
  make && \
  make install && \
  make tools/zmqsend && cp tools/zmqsend ${PREFIX}/bin/ && \
  make distclean && \
  hash -r && \
  cd tools && \
  make qt-faststart && cp qt-faststart ${PREFIX}/bin/


RUN \
  ldd ${PREFIX}/bin/ffmpeg | grep opt/ffmpeg | cut -d ' ' -f 3 | xargs -i cp {} /usr/local/lib/ && \
  cp ${PREFIX}/bin/* /usr/local/bin/ && \
  cp -r ${PREFIX}/share/ffmpeg /usr/local/share/ && \
  LD_LIBRARY_PATH=/usr/local/lib ffmpeg -buildconf

RUN ["rm", "-rf", "/tmp/sources/"]  