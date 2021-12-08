# git-good-pkgbuild
PKGBUILD for git-good

## Prerequisite

You will need `node` and `npm`. Does not work with Node@17 (some openssl changes which are not compatible with nodegit?). Node and npm are not marked as `makedepends` as there are many ways to aquire these: such as `nvm` (or other version managers), system packages or "standalone".

## Installation

Run `makepkg` to build/generate package file. Then install with `pacman -U git-good-[version]-[arch].pkg.tar.zst`
