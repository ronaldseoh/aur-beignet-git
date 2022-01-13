# Maintainer: Michael Lass <bevan@bi-co.net>
# Contributor: Sean Lynch <seanl@literati.org>
# Contributor: Bruno Pagani <archange@archlinux.org>
# Contributor: Antoine Lubineau <antoine@lubignon.info>
# Contributor: Leopold Bloom <blinxwang@gmail.com>
# Contributor: Michal Krenek (a.k.a. Mikos) <m.krenek@gmail.com>

# This PKGBUILD is maintained on github:
# https://github.com/michaellass/AUR

# For me, utests fail when built with Clang 10. I therefore by default stick to
# Clang 7 for which no unofficial patches are required. Patches to build against
# Clang 8, 9 or 10 are however included and can be simply commented in.
# - Michael

pkgname=beignet-git
pkgver=1.2.1.r0.g097365ed
pkgrel=1
pkgdesc="An open source OpenCL implementation for Intel IvyBridge & Haswell iGPUs"
arch=(x86_64)
url="https://01.org/beignet"
license=(LGPL)
depends=(glu clang35 mesa opencl-headers)
makedepends=(git llvm35 cmake ninja python ocl-icd)
provides=(beignet opencl-intel opencl-driver)
conflicts=(beignet)
source=(
    "git+https://github.com/intel/beignet.git"
)
sha256sums=('SKIP')

function pkgver() {
    cd "$srcdir/beignet"
    git describe --long --tags | sed 's/^Release_v//;s/-/.r/;s/-/./'
}

prepare() {
    cd beignet
    git checkout Release_v1.2.1
}

build() {
    cmake -B build -S beignet -G Ninja \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DCMAKE_INSTALL_LIBDIR=/usr/lib \
        -DCMAKE_BUILD_TYPE=RELEASE \
        -DCMAKE_INSTALL_RPATH_USE_LINK_PATH=TRUE
    ninja -C build
}

package() {
    DESTDIR="${pkgdir}" ninja -C build install
    # Remove headers already provided by 'opencl-headers'
    cd "${pkgdir}"/usr/include/CL
    rm cl.h cl_egl.h cl_ext.h cl_gl.h cl_gl_ext.h cl_platform.h opencl.h
}
