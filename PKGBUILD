# Maintainer: Bardia Moshiri <fakeshell@bardia.tech>
# Contributor: Philip Goto <philip.goto@gmail.com>

pkgname=phoc-hybris
provides=('phoc')
conflicts=('phoc')
_pkgbase=phoc
pkgver=0.24.0+1+droidian0
pkgrel=1
_commit=90897e2ef88cdf52b58983241ef79900b4ec0569
pkgdesc="Wlroots based Phone compositor"
url="https://source.puri.sm/Librem5/phoc"
license=("GPL3")
arch=('i686' 'x86_64' 'armv6h' 'armv7h' 'aarch64')
depends=('gobject-introspection' 'gnome-desktop' 'libinput' 'mutter'
         'xcb-util-errors' 'xcb-util-wm' 'wayland-protocols' 'wlroots-hybris' 'ffmpeg')
makedepends=('meson' 'git')
provides=('phoc=0.24.0')
source=("git+https://github.com/droidian/phoc.git#commit=${_commit}")
sha256sums=('SKIP')

pkgver() {
  cd "${srcdir}/${_pkgbase}"
  git describe --tags | sed 's/^v//;s/-/+/g;s|pureos/||g;s|droidian/||g;s|bookworm/||g'
}

prepare() {
  cd "${srcdir}/${_pkgbase}"
}

build() {
  rm -rf build
  arch-meson "${srcdir}/${_pkgbase}" build \
          -Dtests=false \
          -Dembed-wlroots=disabled
  ninja -C build
}

package() {
  DESTDIR="${pkgdir}" ninja -C build install
  install -Dm755 "${srcdir}/${_pkgbase}"/helpers/scale-to-fit ${pkgdir}/usr/bin
}
