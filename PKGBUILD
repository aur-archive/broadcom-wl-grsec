# Maintainer: Steffen Suerbier. <s.suerbier at gmail dot com>

pkgname=broadcom-wl-grsec
pkgver=6.30.223.248
pkgrel=5
pkgdesc='Broadcom 802.11abgn hybrid Linux networking device driver for linux-grsec'
url='http://www.broadcom.com/support/802.11/linux_sta.php'
arch=('i686' 'x86_64')
license=('custom')
depends=('linux-grsec')
makedepends=('linux-grsec-headers')
[[ $CARCH = x86_64 ]] && _arch=_64 || _arch=
source=("http://www.broadcom.com/docs/linux_sta/hybrid-v35${_arch}-nodebug-pcoem-${pkgver//./_}.tar.gz"
        'modprobe.d'
        'license.patch'
        'linux-recent.patch'
        'gcc.patch'
        'grsec.patch')
sha256sums=('b196543a429c22b2b8d75d0c1d9e6e7ff212c3d3e1f42cc6fd9e4858f01da1ad'
            'b4aca51ac5ed20cb79057437be7baf3650563b7a9d5efc515f0b9b34fbb9dc32'
            '2f70be509aac743bec2cc3a19377be311a60a1c0e4a70ddd63ea89fae5df08ac'
            'aa01ee80cd9e7a212500a5f970064132f7df1a7db43269db0306019d4d7be6ca'
            'b07ce80f2e079cce08c8ec006dda091f6f73f158c8a62df5bac2fbabb6989849'
            '00d32d24ee00a2467c1d126b3399181179d24e777b783d513cc0654a8d8efb5f')
[[ $CARCH = x86_64 ]] && sha256sums[0]='3d994cc6c05198f4b6f07a213ac1e9e45a45159899e6c4a7feca5e6c395c3022'

install=install

_kernmajor="$(pacman -Q linux-grsec | awk '{print $2}' | cut -d - -f1 | cut -d . -f1-3)"
_extramodules="extramodules-${_kernmajor}-grsec"
_kernver="$(cat /usr/lib/modules/${_extramodules}/version)"

prepare() {
  cd "${srcdir}"

  patch -p1 -i linux-recent.patch
  patch -p1 -i license.patch
  patch -p1 -i gcc.patch
  patch -p1 -i grsec.patch

  sed -e "/BRCM_WLAN_IFNAME/s:eth:wlan:" \
      -i src/wl/sys/wl_linux.c
}

build() {
  cd "${srcdir}"

  make -C /usr/lib/modules/${_kernver}/build M=`pwd`
}

package() {
  cd "${srcdir}"

  install -Dm644 wl.ko "${pkgdir}/usr/lib/modules/${_extramodules}/wl.ko"
  gzip "${pkgdir}/usr/lib/modules/${_extramodules}/wl.ko"

  install -Dm644 lib/LICENSE.txt "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  install -Dm644 modprobe.d "${pkgdir}/usr/lib/modprobe.d/broadcom-wl-grsec.conf"
}
