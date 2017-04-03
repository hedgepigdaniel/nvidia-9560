# $Id: PKGBUILD 291068 2017-03-19 08:23:40Z andyrtr $
# Maintainer: Eric Bélanger <eric@archlinux.org>

pkgname=nvidia-9560
pkgver=375.39
_extramodules=extramodules-4.9-9560
pkgrel=1
pkgdesc="NVIDIA drivers for linux-lts-9560"
arch=('i686' 'x86_64')
url="http://www.nvidia.com/"
makedepends=("nvidia-utils=$pkgver" 'linux-9560>=4.9.16' 'linux-9560-headers>=4.9.16')
provides=('nvidia')
conflicts=('nvidia-lts')
license=('custom')
install=nvidia-9560.install
options=('!strip')
source_i686=("http://us.download.nvidia.com/XFree86/Linux-x86/${pkgver}/NVIDIA-Linux-x86-${pkgver}.run")
source_x86_64=("http://us.download.nvidia.com/XFree86/Linux-x86_64/${pkgver}/NVIDIA-Linux-x86_64-${pkgver}-no-compat32.run")
md5sums_i686=('9247d5eb9df58bb7755c9060ef5d103d')
md5sums_x86_64=('3541e67b444f020b89bcad8334be6d65')

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
    depends=('linux-9560>=4.4.16' "nvidia-utils-9560=$pkgver" 'libgl')

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
