# Contributor: Łukasz Jendrysik <scadu@yandex.com>
# Maintainer: Natanael Copa <ncopa@alpinelinux.org>
pkgname=sox
pkgver=14.4.2
pkgrel=1
pkgdesc="The Swiss Army knife of sound processing tools"
url="http://sox.sourceforge.net/"
arch="all"
license="GPL LGPL"
makedepends="ffmpeg-dev libao-dev libvorbis-dev libogg-dev lame-dev
	libmad-dev bash alsa-lib-dev libsndfile-dev libsamplerate-dev
	file-dev libid3tag-dev flac-dev gsm-dev opusfile-dev libpng-dev
	autoconf automake libtool
	"
depends=
subpackages="$pkgname-dev $pkgname-doc"
source="http://downloads.sourceforge.net/sourceforge/$pkgname/$pkgname-$pkgver.tar.gz
	sox-uclibc.patch
	sox-dynamic.patch
        sox-logspectrogram.patch
	"

prepare() {
	cd "$srcdir"/$pkgname-$pkgver
	for i in $source; do
		case $i in
		*.patch) msg $i; patch -p1 -i "$srcdir"/$i || return 1;;
		esac
	done
	autoreconf -vif
}

build() {
	cd "$srcdir"/$pkgname-$pkgver
	./configure \
		--build=$CBUILD \
		--host=$CHOST \
		--prefix=/usr \
		--sysconfdir=/etc \
		--with-dyn-default \
		--with-distro="Alpine Linux" \
		|| return 1
	make || return 1
}

package() {
	cd "$srcdir"/$pkgname-$pkgver
	make DESTDIR="$pkgdir" install || return 1
	ln -sf play "$pkgdir"/usr/bin/rec || return 1
	ln -sf ../man1/sox.1.gz "$pkgdir"/usr/share/man/man7/soxeffect.7 || return 1
	rm "$pkgdir"/usr/lib/sox/*.a || return 1
}
sha512sums="b5c6203f4f5577503a034fe5b3d6a033ee97fe4d171c533933e2b036118a43a14f97c9668433229708609ccf9ee16abdeca3fc7501aa0aafe06baacbba537eca  sox-14.4.2.tar.gz
08c55a0de96733e10544d450f39c2205b4057b9fc024503ec97b1906a075752ee8a4b0a1b4c5bbad2eebec17bcf8d069b22d243a63d28b77c23d545efcca6aec  sox-uclibc.patch
3950834db26faa0523006c6fd8e0769d080518f127d345c8ec9bf53e9db8a6bd67cd724f0f86492aaf9ce6ede2dfbde167049768f35c14ef3c2b96e7e00302b6  sox-dynamic.patch
fe753849fbf457216249a2288dbc15b30b505102982b3d41a7e4b856c70075e323a6a5165d085a8d8d6ee1848afda0ac9338bfc6bb326fd45503a7ab333d8cd9  sox-logspectrogram.patch"
