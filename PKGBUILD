# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Philip Goto <philip.goto@gmail.com>

pkgname=phoc
pkgver=0.8.0
pkgrel=1
_commit=be21b6dbb3bc9031d2c8fc5d24542f28faab5b57
_wlroots=0.12.0
pkgdesc="Wlroots based Phone compositor"
url="https://source.puri.sm/Librem5/phoc"
license=("GPL3")
arch=('i686' 'x86_64' 'armv6h' 'armv7h' 'aarch64')
depends=('gobject-introspection' 'gnome-desktop' 'libinput' 'mutter'
         'xcb-util-errors' 'xcb-util-wm')
makedepends=('meson' 'git')
provides=('wlroots')
conflicts=('wlroots')
source=("git+https://gitlab.gnome.org/World/Phosh/${pkgname}.git#commit=${_commit}"
        "wlroots-$_wlroots.tar.gz::https://github.com/swaywm/wlroots/archive/$_wlroots.tar.gz"
        "https://github.com/swaywm/wlroots/releases/download/$_wlroots/wlroots-$_wlroots.tar.gz.sig"
        0001-seat-Don-t-notify-on-key-release.patch
        xcursor-fix-false-positive-stringop-truncation.diff
        Revert-layer-shell-error-on-0-dimension-without-anchors.diff)
sha256sums=('SKIP'
            'c9e9f4f6d2f526d0b2886daf3ec37e64831773059aa669fb98a88522a1626bdb'
            'SKIP'
            '7600bedbed3057c427965668a9bcda433fab57b35726f9ecb07a9328d3dd4238'
            'a6779e7fa2beee02f9949f6c8bd5c279c6d3f9bbc4103230a627f18a5f74761e'
            '544ff25b0a7184ac73cd38ed3c34369721a83ae926016ddb941c4ca21abddac9')
validpgpkeys=(
    '34FF9526CFEF0E97A340E2E40FDE7BE0E88F5E48' # Simon Ser
    '9DDA3B9FA5D58DD5392C78E652CB6609B22DA89A' # Drew DeVault
    '4100929B33EEB0FD1DB852797BC79407090047CA' # Sway signing key
)            

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/^v//;s/-/+/g;s|pureos/||g'
}

prepare() {
  cd $pkgname
  
  rm -r subprojects/wlroots
  mv ../wlroots-$_wlroots subprojects/wlroots

  local src
  for src in "${source[@]}"; do
    src="${src%%::*}"
    src="${src##*/}"
    [[ $src = *.patch ]] || continue
    echo "Applying patch $src..."
    patch -Np1 < "../$src"
  done
  
  cd subprojects/wlroots
  
  for src in "${source[@]}"; do
    src="${src%%::*}"
    src="${src##*/}"
    [[ $src = *.diff ]] || continue
    echo "Applying patch $src..."
    patch -Np1 < "../../../$src"
  done  
}

build() {
  rm -rf build
  arch-meson $pkgname build -Dtests=false \
          -Dwlroots:logind-provider=systemd \
          -Dwlroots:libseat=disabled
  ninja -C build
}

package() {
  DESTDIR="${pkgdir}" ninja -C build install
  install -Dm755 $pkgname/helpers/scale-to-fit ${pkgdir}/usr/bin
}
