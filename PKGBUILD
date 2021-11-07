# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Philip Goto <philip.goto@gmail.com>

pkgname=phoc-hybris
provides=('phoc')
conflicts=('phoc')
_pkgbase=phoc
pkgver=645.81f76ea
pkgrel=2
_commit=81f76ea1a7673e33b5f604e46a80fbe74b87bac6
pkgdesc="Wlroots based Phone compositor"
url="https://source.puri.sm/Librem5/phoc"
license=("GPL3")
arch=('i686' 'x86_64' 'armv6h' 'armv7h' 'aarch64')
depends=('gobject-introspection' 'gnome-desktop' 'libinput' 'mutter'
         'xcb-util-errors' 'xcb-util-wm' 'wayland-protocols' 'wlroots-hybris' 'ffmpeg')
makedepends=('meson' 'git')
provides=('phoc=0.9.0')
source=("git+https://github.com/droidian/phoc.git#commit=${_commit}")
sha256sums=('SKIP')

pkgver() {
  cd "${srcdir}/${_pkgbase}"
  echo $(git rev-list --count HEAD).$(git rev-parse --short HEAD)
}

prepare() {
  cd "${srcdir}/${_pkgbase}"
  
  #rm -r subprojects/wlroots
  #mv ../wlroots-$_wlroots subprojects/wlroots
  
  git submodule init
  git submodule update
}

build() {
  rm -rf build
  meson "${srcdir}/${_pkgbase}" build \
          -Dembed-wlroots=disabled
  ninja -C build
}

package() {
  install -d ${pkgdir}/usr/bin
  DESTDIR="${pkgdir}" ninja -C build install
  install -Dm755 "${srcdir}/${_pkgbase}"/helpers/scale-to-fit ${pkgdir}/usr/bin
}
