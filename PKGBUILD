# Maintainer: weedzcokie
pkgname=git-good
pkgver=v0.1.1.r86.g401ee76
pkgrel=1
pkgdesc='Git-good'
arch=('x86_64')
url='https://github.com/weedz/git-good'
license=('GPL3')
depends=('electron')
makedepends=('git' 'asar')
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
  electron_version=$(</usr/lib/electron/version)
  cd "$pkgname"
  JOBS=max npm exec electron-builder -- --linux --x64 --dir -c.target= -c.npmRebuild=true -c.electronDist=/usr/lib/electron -c.electronVersion="$electron_version"
}

package() {
  install -Dm644 -t "$pkgdir/usr/share/applications" git-good.desktop
  install -Dm755 -t "$pkgdir/usr/bin" git-good-exec
  mv "$pkgdir/usr/bin/git-good-exec" "$pkgdir/usr/bin/git-good"

  cd "$pkgname"

  # Remove unnecessary stuff from app.asar
  asar e out/linux-unpacked/resources/app.asar out/linux-unpacked/resources/app
  rm out/linux-unpacked/resources/app.asar
  rm -rf out/linux-unpacked/resources/app/node_modules/nodegit/vendor
  rm -rf out/linux-unpacked/resources/app/node_modules/nodegit/build/vendor
  rm -rf out/linux-unpacked/resources/app/node_modules/nodegit/include
  rm -rf out/linux-unpacked/resources/app/node_modules/nodegit/lifecycleScripts
  rm -rf out/linux-unpacked/resources/app/node_modules/nodegit/utils

  asar p out/linux-unpacked/resources/app out/linux-unpacked/resources/app.asar --unpack-dir '**'
  rm -rf out/linux-unpacked/resources/app

  install -Dm644 -t "$pkgdir/usr/share/git-good" out/linux-unpacked/resources/app.asar

  # Remove unnecessary stuff from app.asar.unpacked
  rm -rf out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/vendor
  rm -rf out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/include
  rm -rf out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/lifecycleScripts
  rm -rf out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/utils
  rm -rf out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/.github
  rm -rf out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/build/vendor

  rm -f out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/build/Release/git2.a
  rm -f out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/build/Release/http_parser.a
  rm -f out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/build/Release/ntlmclient.a
  rm -f out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/build/Release/pcre.a
  rm -f out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/build/Release/ssh2.a
  rm -f out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/build/Release/zlib.a
  rm -f out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/build/Release/acquireOpenSSL.node
  rm -f out/linux-unpacked/resources/app.asar.unpacked/node_modules/nodegit/build/Release/configureLibssh2.node

  find ./out/linux-unpacked/resources/app.asar.unpacked -type d \( -name linux -o -name mac \) -print -exec rm -r {} +
  cp -rt "$pkgdir/usr/share/git-good" out/linux-unpacked/resources/app.asar.unpacked

  find "$pkgdir" -type d -empty -delete -print
}
