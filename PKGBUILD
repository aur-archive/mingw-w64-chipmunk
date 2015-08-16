# Maintainer: edubart <edub4rt@gmail.com>
pkgname=mingw-w64-chipmunk
pkgver=7.0.0
pkgrel=3
pkgdesc="A high-performance 2D rigid body physics library (mingw-w64)"
arch=(any)
url="http://chipmunk-physics.net/"
license=('MIT')
depends=('mingw-w64-crt')
makedepends=('mingw-w64-gcc' 'mingw-w64-cmake')
options=(!strip !buildflags staticlibs)
source=(http://files.slembcke.net/chipmunk/release/Chipmunk-${pkgver%%.*}.x/Chipmunk-$pkgver.tgz)
md5sums=('87c2c3836036d172b55246854d270e67')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

prepare() {
  sed -i 's/#include <sys\/sysctl.h>//' ${srcdir}/Chipmunk-$pkgver/src/cpHastySpace.c
}

build() {
  unset LDFLAGS

  for _arch in ${_architectures}; do
      mkdir -p ${srcdir}/build-${_arch} && cd ${srcdir}/build-${_arch}
      ${_arch}-cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_DEMOS=OFF \
                     -DCMAKE_C_FLAGS="-DCHIPMUNK_FFI" \
                     ../Chipmunk-$pkgver
      make
  done
}

package() {
  for _arch in ${_architectures}; do
      cd ${srcdir}/build-${_arch}
      make DESTDIR=${pkgdir} install
      ${_arch}-strip -x -g "$pkgdir"/usr/${_arch}/bin/*.dll
      ${_arch}-strip -g "$pkgdir"/usr/${_arch}/lib/*.a
  done
}
