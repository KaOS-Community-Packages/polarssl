pkgname=polarssl
pkgver=1.3.9
pkgrel=2
pkgdesc="Portable cryptographic and SSL/TLS library"
arch=('x86_64')
url="https://www.polarssl.org/"
license=('GPL2')
source=("https://polarssl.org/download/polarssl-$pkgver-gpl.tgz"
        "programs.makefile.patch"
        "CVE-2015-1182.patch")
sha1sums=('3462b4455e1443ac1a1007fbd69861ebfb5c5506'
          '1e9e7d3dcdd6932b02d6dcabdf45041a3726f1be'
          '94317c4757063d006fb4e666b522b581326ba708')
depends=('glibc')
options=('staticlibs')

# http://sources.gentoo.org/cgi-bin/viewvc.cgi/gentoo-x86/net-libs/polarssl/files/polarssl-1.3.8-ssl_pthread_server.patch
# https://github.com/alucryd/aur-alucryd/blob/master/personal/polarssl/PKGBUILD
# https://aur.archlinux.org/packages/po/polarssl-git/PKGBUILD

prepare() {
  cd "$pkgname-$pkgver"
  patch -p1 -i "$srcdir/CVE-2015-1182.patch"
}

build() {
  cd "$pkgname-$pkgver"
  sed -i 's|//\(#define POLARSSL_THREADING_C\)|\1|' include/polarssl/config.h
  sed -i 's|//\(#define POLARSSL_THREADING_PTHREAD\)|\1|' include/polarssl/config.h
  # enable cert_write
  patch -p1 -d programs -i "$srcdir/programs.makefile.patch"
  LDFLAGS+=" -I../include " make SHARED=1 no_test
}

check() {
  cd "$pkgname-$pkgver"
  make SHARED=1 check
}

package() {
  cd "$pkgname-$pkgver"
  make DESTDIR="$pkgdir/usr" install
}
