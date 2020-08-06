# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Philip Goto <philip.goto@gmail.com>

pkgname=phoc
pkgver=0.4.1
pkgrel=3
pkgdesc="Wlroots based Phone compositor"
url="https://source.puri.sm/Librem5/phoc"
license=("GPL3")
arch=('i686' 'x86_64' 'armv6h' 'armv7h' 'aarch64')
depends=('gobject-introspection' 'gnome-desktop' 'libinput' 'mutter'
         'xcb-util-errors' 'xcb-util-wm' 'wlroots')
makedepends=('meson')
source=("https://source.puri.sm/Librem5/phoc/-/archive/v${pkgver}/phoc-v${pkgver}.tar.gz"
        0001-seat-Don-t-notify-on-key-release.patch
        0002-seat-inhibit-touch-events-when-in-power-save-mode-or.patch
        166.patch
        phosh-damage-output-after-rotation.patch)
sha256sums=('420a4e8b1b15a99475fda1221249a7ca7c1f77c09fee114530f2a4612fde84fc'
            'fbabb0c9f31c2520cf72a252e1cc4a4da7091b830ed5e439901f007205a59cd8'
            '6ee0b93d5e2353fef9d5f06b8c7897239de0891daef4c5a02e19f254ab8d256e'
            '4a9d0b47e118d61f1e531064bd1a759f642b54d855e8da27ef7649237d8038d6'
            'ef6679f5d5217b6cb65ec8ed71ac676dfe98de31601512841a7e27481bebac22')

prepare() {
  cd phoc-v${pkgver}
  local src
  for src in "${source[@]}"; do
    src="${src%%::*}"
    src="${src##*/}"
    [[ $src = *.patch ]] || continue
    echo "Applying patch $src..."
    patch -Np1 < "../$src"
  done
}

build() {
  rm -rf build
  arch-meson phoc-v${pkgver} build -Dtests=false -Dembed-wlroots=disabled
  ninja -C build
}

package() {
  DESTDIR="${pkgdir}" ninja -C build install
  install -Dm755 phoc-v${pkgver}/helpers/scale-to-fit ${pkgdir}/usr/bin
}
