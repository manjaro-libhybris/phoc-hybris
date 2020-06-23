# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Philip Goto <philip.goto@gmail.com>

pkgname=phoc
pkgver=0.1.9
pkgrel=1
pkgdesc="Wlroots based Phone compositor"
url="https://source.puri.sm/Librem5/phoc"
license=("GPL3")
arch=('i686' 'x86_64' 'armv6h' 'armv7h' 'aarch64')
depends=(gnome-desktop
         wlroots)
makedepends=(ctags
             libhandy
             meson
             vala)
source=("https://source.puri.sm/Librem5/phoc/-/archive/v${pkgver}/phoc-v${pkgver}.tar.gz")
sha256sums=('520eff0adccb8b560d7938d12ed389bbe93213cae975fb2881a40c3f513169c1')

build() {
    rm -rf build
    arch-meson phoc-v${pkgver} build -Dembed-wlroots=disabled
    ninja -C build
}

package() {
    DESTDIR="${pkgdir}" ninja -C build install
}
