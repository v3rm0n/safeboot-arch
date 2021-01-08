pkgname=safeboot-git
_pkgname=safeboot
pkgver=release.0.6.r220.ge497994
pkgrel=1
pkgdesc='Booting Linux Safely'
arch=('i686' 'x86_64')
url="https://github.com/osresearch/$_pkgname"
license=('GPL2')
makedepends=('git')
depends=('tpm2-tss' 'efitools' 'opensc' 'yubico-piv-tool' 'pkcs11-helper' 'libjson' 'qrencode' 'curl' 'uuid')
provides=("$_pkgname")
conflicts=("$_pkgname")
source=("git+https://github.com/osresearch/$_pkgname")
sha256sums=('SKIP')

pkgver () {
  cd "$srcdir/$_pkgname"
  git describe --long | sed -r 's/([^-]*-g)/r\1/;s/-/./g;s/debian\///g'
}

build () {
  cd "$srcdir/$_pkgname"
  make all
}

package () {
  cd "$srcdir/$_pkgname"

  mkdir -p "$pkgdir"/usr/bin
  
  install sbin/safeboot			"$pkgdir"/usr/bin/
  install sbin/safeboot-tpm-unseal	"$pkgdir"/usr/bin/
  install sbin/tpm2-attest		"$pkgdir"/usr/bin/
  install sbin/tpm2-pcr-validate		"$pkgdir"/usr/bin/

  mkdir -p "$pkgdir"/etc/safeboot/certs/

  install safeboot.conf			"$pkgdir"/etc/safeboot/
  install functions.sh			"$pkgdir"/etc/safeboot/

  install tpm-certs.txt			"$pkgdir"/etc/safeboot/
  install refresh-certs			"$pkgdir"/etc/safeboot/
  install certs/*				"$pkgdir"/etc/safeboot/certs/

  install bin/sbsign.safeboot		"$pkgdir"/usr/bin/
  install bin/sign-efi-sig-list.safeboot	"$pkgdir"/usr/bin/
  install bin/tpm2-totp			"$pkgdir"/usr/bin/
  install bin/tpm2			"$pkgdir"/usr/bin/

  mkdir -p "$pkgdir"/etc/initramfs-tools/hooks
  mkdir -p "$pkgdir"/etc/initramfs-tools/scripts/{local-premount,init-top}

  install initramfs/hooks/dmverity-root	"$pkgdir"/etc/initramfs-tools/hooks/
  install initramfs/hooks/safeboot-hooks	"$pkgdir"/etc/initramfs-tools/hooks/
  install initramfs/scripts/dmverity-root	"$pkgdir"/etc/initramfs-tools/scripts/local-premount/
  install initramfs/scripts/blockdev-readonly "$pkgdir"/etc/initramfs-tools/scripts/local-premount/
  install initramfs/scripts/safeboot-bootmode "$pkgdir"/etc/initramfs-tools/scripts/init-top/
}

# vim: et ts=2 sw=2
