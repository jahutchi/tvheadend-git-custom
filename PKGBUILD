# Maintainer: Francois Menning <f.menning@pm.me>
# Contributor: Dan Ziemba <zman0900@gmail.com>
# Contributor: Benjamin Hedrich <kiwisauce (a) pagenotfound (dot) de>

_gitname='tvheadend-git'
pkgname=tvheadend-git
pkgver=4.3.r2040.g604d81a29
pkgrel=1
pkgdesc="TV streaming server for Linux"
arch=('i686' 'x86_64' 'arm' 'armv6h' 'armv7h' 'aarch64')
url="https://tvheadend.org/"
license=('GPL3')
depends=(
  'avahi'
  'libavresample'
  'libdvbcsa'
  'libfdk-aac'
  'libhdhomerun'
  'libogg'
  'libtheora'
  'libvorbis'
  'libvpx'
  'openssl'
  'opus'
  'pcre2'
  'pngquant'
  'uriparser'
  'x264'
  'x265'
)
makedepends=(
  'git'
  'python'
)
optdepends=(
  'xmltv: For an alternative source of programme listings'
)
options=('!strip' 'emptydirs')
provides=('tvheadend')
conflicts=('tvheadend')
source=(
  "${_gitname}::git+https://github.com/jahutchi/tvheadend.git"
  tvheadend.service
  tmpfile.conf
  user.conf
)
sha512sums=('SKIP'
            '033d41d83e875e2e9d0c88abfa8c5480f203548528aaad21e98771d1abc1e54ed0e5a54dd20f19a3e6b0dfcd27c520ae076f8fba7d85c3d855e455be254bf752'
            'f8139121355cc1f6dc8003897cad0223127648920146c6064c0c70a04295737a8f4d79a670edfc65231bf86b07a42db054f2887bbe50dca8686fd03ac5c79943'
            '78cefaabbe8becd52f40bfceb1105f6fd10be39e09e1259858df8dadadc77870439a403f3f5ddf6f29b078e07d48c03aa05cb5a6200486bc0fb6300d344da348')

pkgver() {
  cd "${srcdir}/${_gitname}"
  git describe --long | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cd "${srcdir}/${_gitname}"
}

build() {
  cd "${srcdir}/${_gitname}"

  # Work-around for GCC 10
  export CFLAGS="$CFLAGS -Wno-error=array-bounds -Wno-error=address"

  ./configure \
    --prefix=/usr \
    --datadir=/var/lib \
    --mandir=/usr/share/man/man1 \
    --python=python3 \
    --enable-avahi \
    --enable-zlib \
    --enable-pngquant \
    --enable-libav \
    --enable-ffmpeg_static \
    --disable-libx264_static --enable-libx264 \
    --disable-libx265_static --enable-libx265 \
    --disable-libvpx_static --enable-libvpx \
    --disable-libogg_static --enable-libogg \
    --disable-libtheora_static --enable-libtheora \
    --disable-libvorbis_static --enable-libvorbis \
    --disable-libfdkaac_static --enable-libfdkaac \
    --disable-libopus_static --enable-libopus \
    --disable-hdhomerun_static --enable-hdhomerun_client

  make
}

package() {
  cd "${srcdir}/${_gitname}"

  make DESTDIR="$pkgdir/" install

  install -Dm 644 "${srcdir}/tvheadend.service" -t "${pkgdir}/usr/lib/systemd/system"
  install -Dm 644 "${srcdir}/user.conf" "${pkgdir}/usr/lib/sysusers.d/tvheadend.conf"
  install -Dm 644 "${srcdir}/tmpfile.conf" "${pkgdir}/usr/lib/tmpfiles.d/tvheadend.conf"
}
