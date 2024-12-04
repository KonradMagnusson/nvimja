# Maintainer: Konrad Magnusson <github dot com slash konradmagnusson>


pkgname=nvimja
pkgver="v1.12.1.d79d8f7"
pkgrel=1
pkgdesc="Modified version of the Ninja build system. Includes baked in RPC communication with neovim using rpclib."
arch=('i686' 'x86_64')
url="https://github.com/konradmagnusson/nvimja"
license=(Apache)
depends=()
makedepends=(git cmake python re2c)
provides=(ninja)
conflicts=(ninja)
source=('git+https://github.com/ninja-build/ninja.git#commit=d79d8f7')
md5sums=(SKIP)


prepare() {
	pushd ninja
	git clone https://github.com/rpclib/rpclib --branch v2.3.0
	patch -Np1 -i ../../ninja-rpc.patch
}

build() {
  cmake \
    -S ninja \
    -B build \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -Wno-dev
  cmake --build build
}

check() {
  cd build
  ./ninja_test
}

package() {
  local site_packages=$(python -c "import site; print(site.getsitepackages()[0])")

  cd "$srcdir/build"
  install -Dm755 -t "$pkgdir/usr/bin" ninja

  cd "$srcdir/ninja"
  install -Dm644 -t "$pkgdir/usr/share/licenses/$pkgname" COPYING
  install -Dm644 -t "$pkgdir/usr/share/doc/ninja" doc/manual.asciidoc

  install -Dm644 -t "$pkgdir/usr/share/vim/vimfiles/syntax" misc/ninja.vim
  install -Dm644 -t "$pkgdir/$site_packages" misc/ninja_syntax.py

  install -Dm644 misc/bash-completion "$pkgdir/usr/share/bash-completion/completions/ninja"
  install -Dm644 misc/zsh-completion "$pkgdir/usr/share/zsh/site-functions/_ninja"
}
