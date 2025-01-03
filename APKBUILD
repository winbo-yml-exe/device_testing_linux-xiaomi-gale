# Reference: <https://postmarketos.org/vendorkernel>
# Kernel config based on: arch/arm64/configs/gale_defconfig

pkgname=linux-xiaomi-gale
pkgver=4.19.191
pkgrel=0
pkgdesc="Xiaomi Redmi 13C kernel fork"
arch="aarch64"
_carch="arm64"
_flavor="xiaomi-gale"
url="https://kernel.org"
license="GPL-2.0-only"
options="!strip !check !tracedeps pmb:cross-native"
makedepends="
	bash
	bc
	bison
	devicepkg-dev
	findutils
	flex
	openssl-dev
	perl
	xz
	dtc
"

# Source
_repository="kernel_xiaomi_gale"
_commit="48f448bbb8dcb87bcd6c60182ed0b63b7934bda6"
_config="config-$_flavor.$arch"
source="
	$pkgname-$_commit.tar.gz::https://github.com/rexix01/$_repository/archive/$_commit.tar.gz
	$_config
	selinux_include_generated_headers.patch
	use_system_cpio.patch
	stop-inlining-blk_crypto_flock-and-ksm_flock.patch
	use_system_dtc.patch
"
builddir="$srcdir/$_repository-$_commit"
_outdir="out"

prepare() {
	default_prepare
	REPLACE_GCCH=0 . downstreamkernel_prepare
}

build() {
	unset LDFLAGS
	make O="$_outdir" ARCH="$_carch" CC="${CC:-gcc}" \
		KBUILD_BUILD_VERSION="$((pkgrel + 1 ))-postmarketOS"
}

package() {
	downstreamkernel_package "$builddir" "$pkgdir" "$_carch" \
		"$_flavor" "$_outdir"

	make dtbs_install O="$_outdir" ARCH="$_carch" \
		INSTALL_DTBS_PATH="$pkgdir"/boot/dtbs
}

sha512sums="
960d1ca371f20370b2bc6081bfddffdaf1e0c5413930cb6f3a955903a20488482279ee561619b27d579ccff3d2dae9c222e867ffd4202227b84aeb3fbc778af2  linux-xiaomi-gale-48f448bbb8dcb87bcd6c60182ed0b63b7934bda6.tar.gz
402a9fe0f0257df1962fd7f2cc1672dad27ff954477bff918adee1f4aed9d1901488f641f355cd5fc5b1d9a750baf4e4c7c842a4dd6b25724a364a4d52466bce  config-xiaomi-gale.aarch64
6ab9db01d35f7f5cc2c19ebe5f65a7dc479a1c68de587300cdde9a6c759d34610666c72f0f321cd450cf56c13df3b54a774e0f7ebdbf0f8608fbfd66b49d04e7  selinux_include_generated_headers.patch
28975f5aac872eab10bdfe2b29a8685b70ddb0d105c6c66a26de88ac912573b430fa20901b65384c9cb99d9740cdff7804cfd95474176f93a5bffbccf8182208  use_system_cpio.patch
e448a1093c09414be36333fbdb0d4a3bc5b59018d571b702c6607cb32927cf1563bf03aa1f2d502e6040490e0b26198dd8204306ebaad41be810ba2d47a2721c  stop-inlining-blk_crypto_flock-and-ksm_flock.patch
c9e562403cd572c66def9adea434731b77617f7561f1ce1079e21e8f02e8fd9cc1febd7e52c581e8e4b1c4aca21c5ca8c5813d2006be13051048d681a640ab3d  use_system_dtc.patch
"
