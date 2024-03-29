pkgname=buildtopia
pkgver=0.1.2
pkgrel=1
pkgdesc="A versatile tool designed for efficiently storing, managing, and building packages and programs. It offers the convenience of templates, empowering users to easily create new crates and targets. With its comprehensive features, buildtopia streamlines the development process and enhances productivity."
arch=('i686' 'x86_64' 'armv7h' 'aarch64')
url="https://github.com/fearlessdots/buildtopia"
license=('GPL3')
depends=('glibc' 'gcc-libs' 'which')
if [ -v TERMUX_BUILD ]; then
	msg2 "Building for Termux"
	makedepends=('golang')
else
	makedepends=('go')
fi
source=("${pkgname}-${pkgver}.tar.gz::${url}/archive/refs/tags/v${pkgver}.tar.gz")
sha256sums=('SKIP')

get_destination_directory() {
	# Destination directory
	if [ -v TERMUX_BUILD ]; then
		_termux_prefix="/data/data/com.termux/files/usr"
		_pkgdestdir="${pkgdir}${_termux_prefix}"
	else
		_pkgdestdir="${pkgdir}/usr"
	fi
}

prepare() {
	cd "${pkgname}-${pkgver}"
	msg2 "Cleaning source"
	make clean
}

build() {
	cd "${pkgname}-${pkgver}"
	msg2 "Building binary and generating program files"
	make build
}

package() {
	msg2 "Getting destination directory"
	get_destination_directory
	echo "Destination directory: ${_pkgdestdir}"

	# Create directories for shell autocompletion
	msg2 "Creating directories for shell autocompletion"
	mkdir -p ${_pkgdestdir}/share/bash-completion/completions ${_pkgdestdir}/share/zsh/site-functions \
		${_pkgdestdir}/share/fish/vendor_completions.d

	cd "${pkgname}-${pkgver}"
	msg2 "Installing binary and program files"
	make install DESTDIR=${_pkgdestdir}
}
