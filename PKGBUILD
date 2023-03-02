# Maintainer: Adam Haydon <adam.haydon@qlik.com>
pkgname=qlik-cli
pkgver=2.19.0
pkgrel=1
pkgdesc="Tool for Qlik Sense SaaS which provides access to all public APIs through the command line."
arch=('x86_64')
url="https://qlik.dev/libraries-and-tools/qlik-cli"
license=('MIT')
groups=()
depends=()
makedepends=(
	"curl"
	"jq"
)
checkdepends=()
optdepends=()
provides=('qlik')
conflicts=()
replaces=()
backup=()
options=()
install=
changelog=
source=()
noextract=()
sha256sums=()
validpgpkeys=()

pkgver() {
	./qlik version | sed -r -n 's/version: ([0-9]+\.[0-9]+\.[0-9]+).*/\1/p'
}

prepare() {
	_release=$(curl https://api.github.com/repos/qlik-oss/${pkgname}/releases/latest -s)
	_pkgver=$(printf "${_release}\n" | jq -r '.tag_name' | sed 's/^v//')
	_release_url=$(printf "${_release}\n" | jq -r '.assets[] | select(.name | match("^qlik-Linux")).browser_download_url')
	_checksum_url=$(printf "${_release}\n" | jq -r '.assets[] | select(.name | match("checksums")).browser_download_url')
	curl -L "${_release_url}" -o "${pkgname/-cli/}-Linux-${_pkgver}-${arch}.tar.gz"
	curl -sL ${_checksum_url} | sed -n "s/${pkgname/-cli/}-Linux-${arch}.tar.gz/${pkgname/-cli/}-Linux-${_pkgver}-${arch}.tar.gz/p" | sha256sum -c
	tar -xzf "${pkgname/-cli/}-Linux-${_pkgver}-${arch}.tar.gz"
}

build() {
	./qlik completion bash > bash
	./qlik completion zsh > zsh
}

package() {
	install -Dm755 "${srcdir}/qlik" "${pkgdir}/usr/bin/qlik"
	install -Dm644 "${srcdir}/bash" "${pkgdir}/usr/share/bash-completion/completions/qlik"
	install -Dm644 "${srcdir}/zsh" "${pkgdir}/usr/share/zsh/site-functions/_qlik"
}
