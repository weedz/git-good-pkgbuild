# Maintainer: weedzcokie
pkgname=git-good
pkgver=0.1.3
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

build() {
  electron_version=$(</usr/lib/electron/version)
  cd "$pkgname"
  JOBS=max npm run electron-rebuild
  JOBS=max npx electron-builder --linux --x64 --dir -c.npmRebuild=false -c.electronDist=/usr/lib/electron -c.electronVersion="$electron_version"
}

package() {
  install -Dm644 -t "$pkgdir/usr/share/applications" git-good.desktop
  install -Dm755 -t "$pkgdir/usr/bin" git-good-exec
  mv "$pkgdir/usr/bin/git-good-exec" "$pkgdir/usr/bin/git-good"

  cd "$pkgname"
  install -Dm644 -t "$pkgdir/usr/share/git-good" out/linux-unpacked/resources/app.asar

  rm -rf out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/vendor
  rm -rf out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/include
  rm -rf out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/lifecycleScripts
  rm -rf out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/utils
  rm -rf out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/.github
  rm -rf out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/build/vendor

  find ./out/linux-unpacked/resources/app.asar.unpacked -type d \( -name linux -o -name mac \) -print -exec rm -r {} +
  cp -rt "$pkgdir/usr/share/git-good" out/linux-unpacked/resources/app.asar.unpacked

  find "$pkgdir" -type d -empty -delete -print
}
