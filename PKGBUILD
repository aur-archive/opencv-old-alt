# Heavily based on 'opencv' by Ray Rashif & Tobias Powalowski
# Maintainer: Holodoc (archlinux@bananapro.de)
# based on build from chrisqeld https://aur.archlinux.org/packages/opencv-old/
pkgname=opencv-old-alt
_pybin=python2
_pydir=python2.7
pkgver=2.1.0
pkgrel=7
pkgdesc="Older version of the Intel(R) Open Source Computer Vision Library, currently required by sikuli-ide"
arch=('i686' 'x86_64')
license=('BSD')
url="http://opencv.willowgarage.com"
depends=('jasper' 'python2-numpy' 'gstreamer0.10-base'
          'gtk2' 'xine-lib' 'libdc1394' 'v4l-utils' )
makedepends=('pkg-config' 'cmake')
options=('!libtool')
source=(http://downloads.sourceforge.net/opencvlibrary/OpenCV-$pkgver.tar.bz2
        libpng15.patch libpng16.patch v4l-mmap.patch
        gcc46.patch linux-2.6.38.patch gcc47.patch)
md5sums=('1d71584fb4e04214c0085108f95e24c8'
         '49e035c4b8502fec740f549c40b6bb1f'
         '058e85ec85c303bdff5becaa786fb87b'
         'c0e2d8ecba3b56974ea2169f61e4905f'
         '0949e4c01bbd942ea0ed13ad877952c8'
         '6e7e4676671ef4e5b5a866f27289cdb0'
         'f5e3d6f38545f15756d086fbdcb32a3f')

build() {
  cd "$srcdir/OpenCV-$pkgver"
  # fix ffmpeg-related C++ issue
  # see http://code.google.com/p/ffmpegsource/source/detail?r=311
  export CXXFLAGS="$CXXFLAGS -D__STDC_CONSTANT_MACROS"


    
  # libpng 1.5 compatibility
  patch -Np1 -i ../libpng15.patch

  # libpng 1.6 compatibility
  patch -Np1 -i ../libpng16.patch


  # fix v4l issue
  patch -Np0 -i ../v4l-mmap.patch

  # fix gcc4.6 issue
  patch -Np1 -i ../gcc46.patch

  # linux 2.6.38 compatibility
  patch -Np1 -i ../linux-2.6.38.patch 

  # gcc47 compability
  patch -Np2 -i ../gcc47.patch 


  cmake . -DCMAKE_BUILD_TYPE=Release \
          -DCMAKE_INSTALL_PREFIX=/opt/opencv-old \
          -DCMAKE_SKIP_RPATH=ON \
          -DWITH_XINE=ON \
          -DWITH_FFMPEG=OFF \
          -DWITH_UNICAP=OFF \
          -DPYTHON_EXECUTABLE=/usr/bin/$_pybin \
          -DPYTHON_INCLUDE_DIR=/usr/include/$_pydir \
          -DPYTHON_LIBRARY=/usr/lib/lib$_pydir.so

  make
}

package() {
  cd "$srcdir/OpenCV-$pkgver"

  make DESTDIR="$pkgdir/" install

  # install license file
  install -Dm644 "$srcdir/OpenCV-$pkgver/doc/license.txt" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  # add opencv-old library path to ld.so.conf
  install -d $pkgdir/etc/ld.so.conf.d
  echo "/opt/opencv-old/lib/" > $pkgdir/etc/ld.so.conf.d/opencv-old.conf
}

# vim:set ts=2 sw=2 et:
