# git-good-pkgbuild
PKGBUILD for [git-good](https://github.com/weedz/git-good)

## Prerequisite

You will need `node` and `npm`. Node and npm are not marked as `makedepends` as there are many ways to aquire these: such as `nvm` (or other version managers), system packages or "standalone".

## Installation

Run `makepkg` (might need `makepkg --cleanbuild` if the cloned repository is acting up..) to build/generate package file. This might take a while as we probably need to compile nodegit. Then install with `pacman -U git-good-[version]-[arch].pkg.tar.zst`
