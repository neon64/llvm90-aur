# Maintainer: Andrew Anderson <andrew.wja@gmail.com>

pkgname=llvm90
pkgdesc="LLVM compiler toolchain, version 9.0.1"
pkgver=9.0.1
pkgrel=7
arch=('x86_64')
url="https://llvm.org/"
license=('custom:University of Illinois/NCSA Open Source License')
depends=('libedit' 'libxml2' 'python2')
makedepends=('cmake' 'libffi' 'libedit' 'ncurses' 'libxml2')
options=('staticlibs')
source=(https://github.com/llvm/llvm-project/releases/download/llvmorg-$pkgver/llvm-$pkgver.src.tar.xz add_limits_header.patch)
sha256sums=('SKIP'
            'c2904df572620b37750bd53edf7a099cb85ae8ab1c87a2c29e13923dbc526b99')

prepare() {
  cd "$srcdir/llvm-$pkgver.src"
  patch --strip=1 --input=${srcdir}/add_limits_header.patch
}

build() {
  mkdir "$srcdir/llvm-$pkgver.src/build"
  cd "$srcdir/llvm-$pkgver.src/build"
  cmake .. \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/opt/llvm90 \
    -DLLVM_HOST_TRIPLE=$CHOST \
    -DLLVM_BUILD_LLVM_DYLIB=ON \
    -DLLVM_LINK_LLVM_DYLIB=ON \
    -DLLVM_INSTALL_UTILS=ON \
    -DLLVM_ENABLE_RTTI=ON \
    -DLLVM_ENABLE_FFI=ON \
    -DLLVM_BUILD_TESTS=OFF \
    -DLLVM_ENABLE_DOXYGEN=OFF \
    -DFFI_INCLUDE_DIR=$(pkg-config --variable=includedir libffi) \
    -DLLVM_BINUTILS_INCDIR=/usr/include
  make -Wno-dev all
}

package() {
  cd "$srcdir/llvm-$pkgver.src/build"
  DESTDIR="$pkgdir" make install

  install -Dm644 ../LICENSE.TXT "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  mkdir -p "$pkgdir/etc/ld.so.conf.d/"
  echo "/opt/llvm90/lib" > "$pkgdir/etc/ld.so.conf.d/70-llvm90.conf"
}
