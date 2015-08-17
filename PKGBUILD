# Maintainer: Alessandro Sagratini <ale_sagra at hotmail dot com>
pkgname=rr62x
pkgver=1.2
pkgrel=1
pkgdesc="Kernel modules for Highpoint RocketRAID 62x SATA/6Gbps Card."
arch=('i686' 'x86_64')
url="http://www.highpoint-tech.com/USA_new/series_rr600-download.htm"
license=('custom')
groups=()

depends=('linux')
makedepends=('linux-headers')

provides=()
conflicts=()
replaces=()
backup=()
options=()
install=$pkgname.install
source=(http://www.highpoint-tech.com/BIOS_Driver/rr62x/linux/rr62x-linux-src-v1.2-120601-1355.tar.gz linux_4.patch)
noextract=()
md5sums=('b308bcba65d065cddf05d9b7dea8f3d6' 'd4fbdc757b8e061410396f0c829958f9')
_extramodules=$(basename /lib/modules/extramodules-*)
_kernver=$(cat /lib/modules/$_extramodules/version)

build() {
    if [ ! -e /lib/modules/$_kernver/build/ ]; then
      echo Cannot find kernel souce code from /lib/modules/$_kernver/build/
      exit 1
    fi

    # Apply the Linux version 4 patch
    patch -p0 -i $startdir/linux_4.patch

    rm -rf $startdir/pkg/
    mkdir $startdir/pkg/

    cd rr62x-linux-src-v1.2
    cd product/rr62x/linux/
    make || return 1
}
 
package() {
    cd rr62x-linux-src-v1.2
    cd product/rr62x/linux/

    # Install the kernel module
    mkdir -p "${pkgdir}/usr/lib/modules/${_extramodules}/"
    install -m 644 -D rr62x.ko "${pkgdir}/usr/lib/modules/${_extramodules}/"
    gzip "${pkgdir}/usr/lib/modules/${_extramodules}/rr62x.ko"

    mkdir -p $pkgdir/usr/share/licenses/$pkgname
    cp $srcdir/rr62x-linux-src-v$pkgver/README $pkgdir/usr/share/licenses/$pkgname/
}
