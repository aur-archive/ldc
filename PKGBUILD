# $Id: PKGBUILD 113508 2014-06-24 12:11:38Z dicebot $
# Maintainer: Mihails Strasuns <public@dicebot.lv>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
pkgname=('ldc' 'liblphobos-devel')
groups=('dlang' 'dlang-ldc')
pkgver=0.14.0
epoch=1
pkgrel=1
pkgdesc="A D Compiler based on the LLVM Compiler Infrastructure including D runtime and libphobos2"
arch=('i686' 'x86_64')
url="https://github.com/ldc-developers/ldc"
license=('BSD')
depends=('libconfig')
makedepends=('git' 'cmake' 'llvm')
source=("git://github.com/ldc-developers/ldc.git#tag=v${pkgver}-alpha1"
        "ldc2.conf"
        "ldc2.rebuild.conf"
       )
sha1sums=('SKIP'
          'SKIP'
          'SKIP'
         )

build() {
    cd $srcdir/ldc

    git submodule update --init --recursive

    mkdir build && cd build
    cmake \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DINCLUDE_INSTALL_DIR=/usr/include/dlang/ldc \
    -DBUILD_SHARED_LIBS=ON ..
    make
}

package_ldc() {
    depends=('liblphobos-devel')
    backup=("etc/ldc2.conf"
            "etc/ldc2.rebuild.conf"
           )
    provides=("d-compiler")

    # binaries
    install -D -m755 $srcdir/ldc/build/bin/ldmd2 $pkgdir/usr/bin/ldmd2 
    install -D -m755 $srcdir/ldc/build/bin/ldc2 $pkgdir/usr/bin/ldc2 

    # supplementaries
    install -D -m644 $srcdir/ldc/bash_completion.d/ldc $pkgdir/usr/share/bash-completion/completions/ldc

    # licenses
    install -D -m644 $srcdir/ldc/LICENSE $pkgdir/usr/share/licenses/$pkgname/LICENSE    

    # default configuration files
    install -D -m644 $srcdir/ldc2.conf $pkgdir/etc/ldc2.conf
    install -D -m644 $srcdir/ldc2.rebuild.conf $pkgdir/etc/ldc2.rebuild.conf
}

package_liblphobos-devel() {
    provides=("d-runtime" "d-stdlib")
	options=("staticlibs")

    # libraries
    install -D -m644 $srcdir/ldc/build/lib/libphobos2-ldc.so $pkgdir/usr/lib/liblphobos2.so
    install -D -m644 $srcdir/ldc/build/lib/libdruntime-ldc.so $pkgdir/usr/lib/libldruntime.so
    install -D -m644 $srcdir/ldc/build/lib/libphobos2-ldc-debug.so $pkgdir/usr/lib/liblphobos2-debug.so
    install -D -m644 $srcdir/ldc/build/lib/libdruntime-ldc-debug.so $pkgdir/usr/lib/libldruntime-debug.so

    # imports
    mkdir -p $pkgdir/usr/include/dlang/ldc
    cp -r $srcdir/ldc/build/import/* $pkgdir/usr/include/dlang/ldc/
    cp -r $srcdir/ldc/runtime/phobos/std $pkgdir/usr/include/dlang/ldc/
    cp -r $srcdir/ldc/runtime/phobos/etc $pkgdir/usr/include/dlang/ldc/

    # licenses
    install -D -m644 $srcdir/ldc/LICENSE $pkgdir/usr/share/licenses/$pkgname/LICENSE    
}
