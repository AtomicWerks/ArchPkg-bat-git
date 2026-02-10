# Maintainer: ramen <hendrik@hndrkk.sh>
# Contributor: Vincent Kobel (v@kobl.one)

_pkgname='bat'
pkgname="bat-cat-git"
pkgver=r3705.cb8b6375
pkgrel=1
pkgdesc="A cat(1) clone with wings."
arch=('x86_64')
url='https://github.com/sharkdp/bat'
license=('Apache-2.0')
makedepends=('git' 'rust' 'clang' 'bash' 'sed')
options=('!lto')
provides=('bat')
conflicts=('bat')
source=("git+https://github.com/sharkdp/${_pkgname}")
sha256sums=('SKIP')

pkgver() {
    cd "${srcdir}/${_pkgname}"
    printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  cd "${srcdir}/${_pkgname}"
  cargo build --release
  sed -i -e "s/^bat/.\/target\/release\/${_pkgname}/g" assets/create.sh
  sed -i -e "s/submodule_prompt=unspecified/submodule_prompt=yes/g" assets/create.sh
  bash assets/create.sh
}

package() {
  cd "${srcdir}/${_pkgname}"
  depends+=(libgit2.so)
  install -D ./target/release/${_pkgname} "${pkgdir}/usr/bin/${_pkgname}"
  # Package licenses
  install -Dm644 -t "$pkgdir/usr/share/licenses/$pkgname/" LICENSE-APACHE LICENSE-MIT

  cd target/release/build

  # Find and package the man page (because cargo --out-dir is too new)
  find . -name bat.1 -type f -exec install -Dm644 {} \
    "$pkgdir/usr/share/man/man1/bat.1" \;

  # Find and package the bash completion file
  find . -name bat.bash -type f -exec install -Dm644 {} \
    "$pkgdir/usr/share/bash-completion/completions/bat" \;

  # Find and package the zsh completion file (not in zsh-completions yet)
  find . -name bat.zsh -type f -exec install -Dm644 {} \
    "$pkgdir/usr/share/zsh/site-functions/_bat" \;

  # Find and package the fish completion file
  find . -name bat.fish -type f -exec install -Dm644 {} \
    "$pkgdir/usr/share/fish/vendor_completions.d/bat.fish" \;

}

