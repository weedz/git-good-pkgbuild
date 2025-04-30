# Maintainer: weedzcokie
pkgname=git-good
pkgver=v0.1.5.r13.g08fa6bd
pkgrel=1
pkgdesc='Git-good'
arch=('x86_64')
url='https://github.com/weedz/git-good'
license=('GPL3')
depends=(
  'electron35'
  'openssl'
  'krb5'
)
makedepends=('git')
provides=('git-good')
source=(git-good.tar.bz2
  git-good-exec
  git-good.desktop)
sha256sums=('SKIP' 'SKIP' 'SKIP')

prepare() {
  cd "$pkgname"
  # pnpm install --ignore-scripts
  pnpm run build
}

pkgver() {
  cd "$pkgname"
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
  cd "$pkgname"
  ./scripts/build-native.sh
}

package() {
  install -Dm644 -t "$pkgdir/usr/share/applications" git-good.desktop
  install -Dm755 -t "$pkgdir/usr/bin" git-good-exec
  mv "$pkgdir/usr/bin/git-good-exec" "$pkgdir/usr/bin/git-good"

  cd "$pkgname"

  install -Dm644 -t "$pkgdir/usr/share/git-good/app" package.json
  cp -rt "$pkgdir/usr/share/git-good/app" dist

  # nodegit
  install -Dm644 -t "$pkgdir/usr/share/git-good/app/node_modules/nodegit" node_modules/nodegit/package.json
  install -Dm644 -t "$pkgdir/usr/share/git-good/app/node_modules/nodegit" node_modules/nodegit/LICENSE
  mkdir -p "$pkgdir/usr/share/git-good/app/node_modules/nodegit"
  cp -rt "$pkgdir/usr/share/git-good/app/node_modules/nodegit" node_modules/nodegit/lib
  install -Dm644 -t "$pkgdir/usr/share/git-good/app/node_modules/nodegit/build/Release" node_modules/nodegit/build/Release/nodegit.node

  find "$pkgdir" -type d -empty -delete -print
}
