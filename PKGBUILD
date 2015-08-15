# Maintainer: DrZaius <lou[at]fakeoutdoorsman[dot]com>

pkgname=ffmpeg-vdpau-vaapi
pkgver=20121229
pkgrel=2
pkgdesc="Record, convert, and stream audio and video"
arch=('i686' 'x86_64')
url="http://ffmpeg.org/"
license=('GPL' 'custom')
depends=('alsa-lib' 'bzip2' 'faac' 'lame' 'libtheora' 'libva' 'libvdpau' 'libvorbis' 'libxfixes' 'sdl' 'x264' 'zlib' 'gsm' 'rtmpdump' 'v4l-utils' 'gnutls')
makedepends=('git' 'yasm')
conflicts=('ffmpeg')
replaces=('ffmpeg')
provides=('ffmpeg' "ffmpeg=$pkgver" "qt-faststart")

_gitroot="git://git.videolan.org/ffmpeg"
_gitname="ffmpeg"

build() {
  cd "$srcdir"
  msg "Connecting to the Git repository..."
  
  if [[ -d "$srcdir/$_gitname" ]] ; then
    cd "$_gitname"
    git pull origin
    msg "The local files are updated"
  else
    git clone --depth 1 $_gitroot
  fi
  
  msg "Starting make"
  
  rm -rf "$srcdir/$_gitname-build"
  cp -a "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  
  cd "$srcdir/$_gitname-build"

  ./configure \
    --prefix=/usr \
    --enable-gnutls \
    --enable-libmp3lame \
    --enable-libvorbis \
    --enable-libxvid \
    --enable-libx264 \
    --enable-libvpx \
    --enable-libtheora \
    --enable-libgsm \
    --enable-libspeex \
    --enable-postproc \
    --enable-shared \
    --enable-x11grab \
    --enable-libopencore_amrnb \
    --enable-libopencore_amrwb \
    --enable-libschroedinger \
    --enable-libopenjpeg \
    --enable-librtmp \
    --enable-libpulse \
    --enable-libv4l2 \
    --enable-gpl \
    --enable-version3 \
    --enable-runtime-cpudetect \
    --disable-debug \
    --enable-nonfree \
    --enable-libfaac \
    --enable-vdpau \
    --enable-vaapi \
    --disable-static

 
  make
  make tools/qt-faststart
  make doc/ff{mpeg,play,probe,server}.1
}

package() {
  cd "$srcdir/$_gitname-build"
  make DESTDIR="$pkgdir" install install-man
  install -D -m755 tools/qt-faststart "$pkgdir/usr/bin/qt-faststart" 
  rm -rf "$srcdir/$_gitname-build"
}
