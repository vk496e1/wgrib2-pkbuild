# Contributor: Anton Bazhenov <anton.bazhenov at gmail>
# Contributor: Graziano Giuliani <giuliani@lamma.rete.toscana.it>

pkgname=wgrib2
pkgver=2.0.7
pkgrel=1
pkgdesc="A program to manipulate, inventory and decode GRIB-2 files"
arch=('i686' 'x86_64')
url="http://www.cpc.noaa.gov/products/wesley/wgrib2/"
license=('custom')
depends=('netcdf' 'jasper' 'libpng' 'libmariadbclient' 'proj' 'libaec' 'gcc-fortran')
makedepends=('g2clib' 'gctpc')
source=(ftp://ftp.cpc.ncep.noaa.gov/wd51we/${pkgname}/${pkgname}_nolib.tgz.v${pkgver}
        wgrib2.patch
	      proj_deprecated_api.patch
        http://www.ftp.cpc.ncep.noaa.gov/wd51we/${pkgname}/iplib_hwrf.tgz)
sha256sums=(
  'e74b2a523e4aa54576746fce89fd2f28ec8a483fcbf55e36ed08ed1bb7673e77'
  '8b6d1e9de7cd288f73702fef2065c52383d117832c8ca434fd954668d3297fca'
  '90f1d6176deb04d5b59afb13647cd1bbddb41c8b9bf37a39c70e4ad75ae56dcd'
  'fe2219b1a7909c59ea9b406bf4cc58ce3df7f992bb192cb3ce4fa7da6f93cf6c'
         )

prepare() {
  cd ${srcdir}/grib2
  patch -p0 -i ${srcdir}/wgrib2.patch
  sed -i 's/image.inmem_.*=.*1;//' wgrib2/enc_jpeg2000_clone.c

  patch -p1 -i ${srcdir}/proj_deprecated_api.patch
}

build() {
  cd ${srcdir}/iplib_hwrf
  rm -f *.o *.a
  FC=gfortran FFLAGS='${CFLAGS} -fPIC -DPIC' make
  cd ${srcdir}/grib2
  FC=gfortran F90=gfortran F77=gfortran make
}

package()
{
  cd ${srcdir}/grib2
  install -Dm755 ${pkgname}/${pkgname} ${pkgdir}/usr/bin/${pkgname}
  install -Dm644 ${pkgname}/LICENSE-${pkgname} \
    ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
}
