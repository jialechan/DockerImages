FROM jialechan/alpine-scratch-ffmpeg:1.1.2

RUN apk -Uuv add openjdk8-jre && rm /var/cache/apk/* 

ENV PKGNAME=graphicsmagick
ENV PKGVER=1.3.35
ENV PKGSOURCE=http://downloads.sourceforge.net/$PKGNAME/$PKGNAME/$PKGVER/GraphicsMagick-$PKGVER.tar.lz

# add repo,latest alpine don't have this jasper-dev
RUN echo 'http://dl-cdn.alpinelinux.org/alpine/v3.10/main' >> /etc/apk/repositories

# Installing graphicsmagick dependencies
RUN apk add --no-cache --update g++ \
  gcc \
  make \
  automake \
  autoconf \
  git \
  lzip \
  wget \
  sdl-dev \
  giflib-dev \
  libjpeg-turbo-dev \
  lcms2-dev \
  libwebp-dev \
  libwmf-dev \
  jasper-dev \
  libx11-dev \
  libpng-dev \
  libtool \
  jasper-dev \
  bzip2-dev \
  libxml2-dev \
  tiff-dev \
  exiftool \
  freetype-dev \
  libgomp

RUN wget $PKGSOURCE --no-check-certificate && \
  lzip -d -c GraphicsMagick-$PKGVER.tar.lz | tar -xvf - && \
  cd GraphicsMagick-$PKGVER && \
  ./configure \
  --build=$CBUILD \
  --host=$CHOST \
  --prefix=/usr \
  --sysconfdir=/etc \
  --mandir=/usr/share/man \
  --infodir=/usr/share/info \
  --localstatedir=/var \
  --enable-shared \
  --disable-static \
  --with-modules \
  --with-threads \
  --with-webp=yes \
  --with-tiff=yes \
  --with-jpeg=yes \
  --with-jp2=yes \
  --with-xml=yes \
  --with-wmf=yes \
  --with-gs-font-dir=/usr/share/fonts/Type1 \
  --with-quantum-depth=16 && \
  make && \
  make install