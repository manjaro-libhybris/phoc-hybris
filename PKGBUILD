# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Philip Goto <philip.goto@gmail.com>

pkgname=phoc
pkgver=0.1.9
pkgrel=2
pkgdesc="Wlroots based Phone compositor"
url="https://source.puri.sm/Librem5/phoc"
license=("GPL3")
arch=('i686' 'x86_64' 'armv6h' 'armv7h' 'aarch64')
depends=('gobject-introspection' 'gnome-desktop' 'libinput' 'mutter'
         'xcb-util-errors' 'xcb-util-wm' 'wlroots')
makedepends=('meson')
source=("https://source.puri.sm/Librem5/phoc/-/archive/v${pkgver}/phoc-v${pkgver}.tar.gz")
sha256sums=('520eff0adccb8b560d7938d12ed389bbe93213cae975fb2881a40c3f513169c1')

build() {
    rm -rf build
    arch-meson phoc-v${pkgver} build -Dtests=false -Dembed-wlroots=disabled
    ninja -C build
}

package() {
    DESTDIR="${pkgdir}" ninja -C build install
}
