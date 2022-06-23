# Maintainer: weedzcokie
pkgname=git-good
pkgver=v0.1.1.r117.g86a9a54
pkgrel=1
pkgdesc='Git-good'
arch=('x86_64')
url='https://github.com/weedz/git-good'
license=('GPL3')
depends=('electron')
makedepends=('git')
provides=('git-good')
source=("$pkgname::git+$url.git"
        git-good-exec
        git-good.desktop)
sha256sums=('SKIP' 'SKIP' 'SKIP')

prepare() {
  cd "$pkgname"
  npm install --no-audit --no-progress --no-fund --ignore-scripts
  npm run build
}

pkgver() {
  cd "$pkgname"
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
  electron_version=$(electron -v | sed 's/v//')
  cd "$pkgname"
  JOBS=max npm exec electron-builder -- --linux --x64 --dir -c.target= -c.npmRebuild=true -c.electronVersion="$electron_version"
}

package() {
  install -Dm644 -t "$pkgdir/usr/share/applications" git-good.desktop
  install -Dm755 -t "$pkgdir/usr/bin" git-good-exec
  mv "$pkgdir/usr/bin/git-good-exec" "$pkgdir/usr/bin/git-good"

  cd "$pkgname"

  install -Dm644 -t "$pkgdir/usr/share/git-good" out/linux-unpacked/resources/app.asar

  cp -rt "$pkgdir/usr/share/git-good" out/linux-unpacked/resources/app.asar.unpacked

  find "$pkgdir" -type d -empty -delete -print
}
