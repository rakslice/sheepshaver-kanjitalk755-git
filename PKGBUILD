# Maintainer: Eric DeStefano <eric at ericdestefano dot com>

pkgbase=sheepshaver-kanjitalk755-git
pkgname=(sheepshaver-kanjitalk755-git sheepnet-dkms-kanjitalk755-git)
pkgver=r2953.g607f4ed3
pkgrel=1
pkgdesc="An Open Source PowerMac Emulator"
arch=('x86_64')
url="http://sheepshaver.cebix.net"
license=('GPL')
makedepends=('git' 'dkms')
depends=('gtk2' 'sdl2')
source=('git+https://github.com/kanjitalk755/macemu'
        'SheepShaver.sysctl'
        'SheepShaver.desktop'
        'SheepShaver.png')
sha256sums=('SKIP'
            'a4aa858b95d29906873693988d5db42d5a4da8aa94a72c79374f59fc488efd51'
            '31d9d53f1532a6e4258cb48ba2e638962f640c6401e4a5bfe4b59bd568eab74b'
            'b7f67b1f8424f3e0ffa1a5e57597f368c4c4f93ea1f871ec0a76700b7519b241')

pkgver() {
  cd macemu
  echo "r$(git rev-list --count HEAD).g$(git rev-parse --short HEAD)"
}



build() {
  cd macemu/SheepShaver/src/Unix
  ./autogen.sh \
    --prefix=/usr \
    --enable-standalone-gui \
    --enable-sdl-audio \
    --enable-sdl-video \
    --with-bincue \
    ;
  make -j1
}

package_sheepshaver-kanjitalk755-git() {
  provides=("sheepshaver=$pkgver")
  conflicts=("sheepshaver")

  install -Dm755 macemu/SheepShaver/src/Unix/SheepShaver    "$pkgdir"/usr/bin/SheepShaver
  install -Dm755 macemu/SheepShaver/src/Unix/SheepShaverGUI "$pkgdir"/usr/bin/SheepShaverGUI

  mkdir -p "$pkgdir"/usr/share/doc
  cp -a macemu/SheepShaver/doc/Linux "$pkgdir"/usr/share/doc/SheepShaver

  install -Dm644 SheepShaver.desktop "$pkgdir"/usr/share/applications/SheepShaver.desktop
  install -Dm644 SheepShaver.png     "$pkgdir"/usr/share/pixmaps/SheepShaver.png
  install -Dm644 SheepShaver.sysctl  "$pkgdir"/etc/sysctl.d/90-SheepShaver.conf
}

package_sheepnet-dkms-kanjitalk755-git() {
  depends=('dkms')
  provides=("sheepnet-dkms=$pkgver")
  conflicts=("sheepnet-dkms")

  mkdir -p "$pkgdir"/usr/src
  cp -rL macemu/SheepShaver/src/Unix/Linux/NetDriver "$pkgdir"/usr/src/sheepnet-$pkgver

  cat > "$pkgdir"/usr/src/sheepnet-$pkgver/dkms.conf <<EOF
PACKAGE_NAME="sheepnet"
PACKAGE_VERSION="$pkgver"
AUTOINSTALL=yes
BUILT_MODULE_NAME="sheep_net"
DEST_MODULE_LOCATION="/kernel/net"
EOF
}

# vim: ts=2:sw=2:et:
