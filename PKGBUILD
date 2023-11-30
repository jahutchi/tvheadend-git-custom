# Maintainer: willemw <willemw12@gmail.com>
# Contributor: Francois Menning <f.menning@pm.me>
# Contributor: Dan Ziemba <zman0900@gmail.com>
# Contributor: Benjamin Hedrich <kiwisauce (a) pagenotfound (dot) de>

pkgname=tvheadend-git
pkgver=4.3.r2167.g2d8cf76
pkgrel=1
pkgdesc='TV streaming server and DVR'
#arch=(x86_64)
arch=(aarch64 arm armv6h armv7h i686 x86_64)
url=https://tvheadend.org/
license=(GPL3)
depends=(avahi ffmpeg libdvbcsa libfdk-aac libhdhomerun libogg libtheora libvorbis libvpx
         openssl opus pcre2 pngquant uriparser x264 x265)
makedepends=(git python)
optdepends=('xmltv: alternative source of programme listings')
# NOTE: !lto : avoid build error. See https://tvheadend.org/issues/6026
options=(!lto !strip emptydirs)
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
source=("$pkgname::git+https://github.com/jahutchi/tvheadend.git"
        tmpfile.conf
        tvheadend.service
        user.conf)
sha256sums=('SKIP'
            'b5682442484f5604b5be8d21b186c39187ac4792540f298dbbb7bc9d952b0135'
            'eb1d4fdbd51a48c997d08dc43c732dee6099c170e0633a010066402e6ad5a8a3'
            'e513b752ff665ef917d33df12d278c3efd85e814eadfc8974d540f16e9e2c0d5')

pkgver() {
  git -C $pkgname describe --long --abbrev=7 | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
  cd $pkgname

  ./configure \
    --datadir=/var/lib \
    --disable-ffmpeg_static \
    --disable-hdhomerun_static \
    --disable-libfdkaac_static \
    --disable-libogg_static \
    --disable-libopus_static \
    --disable-libtheora_static \
    --disable-libvorbis_static \
    --disable-libvpx_static \
    --disable-libx264_static \
    --disable-libx265_static \
    --enable-avahi \
    --enable-libav \
    --enable-pngquant \
    --enable-vaapi \
    --enable-zlib \
    --mandir=/usr/share/man/man1 \
    --prefix=/usr \
    --python=python3

  make
}

package() {
  make -C $pkgname DESTDIR="$pkgdir/" install

  install -Dm644 tmpfile.conf         "$pkgdir/usr/lib/tmpfiles.d/tvheadend.conf"
  install -Dm644 tvheadend.service -t "$pkgdir/usr/lib/systemd/system"
  install -Dm644 user.conf            "$pkgdir/usr/lib/sysusers.d/tvheadend.conf"
}
