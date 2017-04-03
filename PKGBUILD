# $Id: PKGBUILD 291068 2017-03-19 08:23:40Z andyrtr $
# Maintainer: Eric Bélanger <eric@archlinux.org>

pkgname=nvidia-lts
pkgver=378.13
_extramodules=extramodules-4.9-lts
pkgrel=4
pkgdesc="NVIDIA drivers for linux-lts"
arch=('i686' 'x86_64')
url="http://www.nvidia.com/"
makedepends=("nvidia-utils=$pkgver" 'linux-lts>=4.9.16' 'linux-lts-headers>=4.9.16')
provides=('nvidia')
license=('custom')
install=nvidia-lts.install
options=('!strip')
source_i686=("http://us.download.nvidia.com/XFree86/Linux-x86/${pkgver}/NVIDIA-Linux-x86-${pkgver}.run")
source_x86_64=("http://us.download.nvidia.com/XFree86/Linux-x86_64/${pkgver}/NVIDIA-Linux-x86_64-${pkgver}-no-compat32.run")
md5sums_i686=('dd1077750af9a067739ec291fb24175f')
md5sums_x86_64=('fe4d25b19a780a690cafc8e3b7c0113f')

[[ "$CARCH" = "i686" ]] && _pkg="NVIDIA-Linux-x86-${pkgver}"
[[ "$CARCH" = "x86_64" ]] && _pkg="NVIDIA-Linux-x86_64-${pkgver}-no-compat32"

prepare() {
    sh ${_pkg}.run --extract-only
}

build() {
    export IGNORE_CC_MISMATCH=1

    _kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
    cd "${_pkg}/kernel"
    make SYSSRC=/usr/lib/modules/${_kernver}/build module
}

package() {
    depends=('linux-lts>=4.4.16' "nvidia-utils=$pkgver" 'libgl')

    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia.ko" \
      "${pkgdir}/usr/lib/modules/${_extramodules}/kernel/drivers/video/nvidia.ko"
    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia-modeset.ko" \
      "${pkgdir}/usr/lib/modules/${_extramodules}/kernel/drivers/video/nvidia-modeset.ko"
    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia-drm.ko" \
      "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia-drm.ko"

    if [[ "$CARCH" = "x86_64" ]]; then
      install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia-uvm.ko" \
        "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia-uvm.ko"
    fi

    gzip "${pkgdir}/usr/lib/modules/${_extramodules}/kernel/drivers/video/"*.ko
    install -d -m755 "${pkgdir}/usr/lib/modprobe.d"
    echo "blacklist nouveau" >> "${pkgdir}/usr/lib/modprobe.d/nvidia-lts.conf"
    echo "blacklist nvidiafb" >> "${pkgdir}/usr/lib/modprobe.d/nvidia-lts.conf"
    install -D -m644 "${srcdir}/${_pkg}/LICENSE" "${pkgdir}/usr/share/licenses/nvidia-lts/LICENSE"
}
