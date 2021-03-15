# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Philip Goto <philip.goto@gmail.com>

pkgname=phoc
pkgver=0.6.0+10+gfcdd0e1
pkgrel=2
_commit=fcdd0e1481b6666283dc4c19579ff0c284f0b86d
pkgdesc="Wlroots based Phone compositor"
url="https://source.puri.sm/Librem5/phoc"
license=("GPL3")
arch=('i686' 'x86_64' 'armv6h' 'armv7h' 'aarch64')
depends=('gobject-introspection' 'gnome-desktop' 'libinput' 'mutter'
         'xcb-util-errors' 'xcb-util-wm')
makedepends=('meson' 'git')
provides=('wlroots')
conflicts=('wlroots')
source=("git+https://source.puri.sm/Librem5/${pkgname}.git#commit=${_commit}"
        0001-seat-Don-t-notify-on-key-release.patch
        0002-seat-inhibit-touch-events-when-in-power-save-mode-or.patch
        https://source.puri.sm/Librem5/phoc/-/merge_requests/242.patch
        https://source.puri.sm/Librem5/phoc/-/merge_requests/243.patch)
sha256sums=('SKIP'
            'fbabb0c9f31c2520cf72a252e1cc4a4da7091b830ed5e439901f007205a59cd8'
            '6dce25b600e9b3217a5de2c384c87eeba4b022b460a2c071e57d2830e7545727'
            'c474329edd4e45d6b342609ab8ca3878e47e14345ca47e9e1354633107e0c6b2'
            'ec02d932549adc94645dcb07ae586909257afa48b28ece95e38dbbed92614a31')

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/^v//;s/-/+/g;s|pureos/||g'
}

prepare() {
  cd $pkgname

  # update the submodules
  git submodule update --init --recursive

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
  arch-meson $pkgname build -Dtests=false
  ninja -C build
}

package() {
  DESTDIR="${pkgdir}" ninja -C build install
  install -Dm755 $pkgname/helpers/scale-to-fit ${pkgdir}/usr/bin
}
