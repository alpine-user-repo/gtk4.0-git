pkgname=gtk4.0-git
pkgver=_git
pkgrel=0
pkgdesc="The GTK Toolkit (v4)"
url="https://www.gtk.org/"
install="$pkgname.post-install $pkgname.post-upgrade $pkgname.post-deinstall"
arch="all !mips !mips64 !s390x !riscv64" # blocked by polkit -> colord
options="!check" # Test suite is known to fail upstream
license="LGPL-2.1-or-later"
subpackages="$pkgname-demo $pkgname-dev $pkgname-doc $pkgname-lang $pkgname-dbg"
depends="shared-mime-info gtk-update-icon-cache"

depends_dev="
	atk-dev
	gdk-pixbuf-dev
	glib-dev
	libepoxy-dev
	libxext-dev
	libxi-dev
	libxinerama-dev
	wayland-protocols
	wayland-libs-client
	wayland-libs-cursor
	libxkbcommon-dev
	vulkan-headers
	"
makedepends="
	$depends_dev
	perl
	cups-dev
	expat-dev
	gettext-dev
	gnutls-dev
	gobject-introspection-dev
	libice-dev
	tiff-dev
	zlib-dev
	at-spi2-atk-dev
	cairo-dev
	fontconfig-dev
	pango-dev
	wayland-dev
	libx11-dev
	libxcomposite-dev
	libxcursor-dev
	libxdamage-dev
	libxfixes-dev
	libxrandr-dev
	meson
	iso-codes-dev
	vulkan-loader-dev
	sassc
	colord-dev
	gstreamer-dev
	gst-plugins-bad-dev
	gtk-doc
	graphene-dev
	pango-git-dev
	"

source=""

builddir="$srcdir/$pkgname"

prepare() {
    default_prepare
    cd "$srcdir"
    git clone --depth=1 "https://gitlab.gnome.org/GNOME/gtk.git" "$pkgname"
}

build() {
    	cd "$builddir"
	# We don't run tests and they currently fail to build
	abuild-meson \
		-Dbroadway-backend=true \
		-Dman-pages=true \
		-Dbuild-tests=false \
		. output
	meson compile ${JOBS:+-j ${JOBS}} -C output
}

package() {
	DESTDIR="$pkgdir" meson install --no-rebuild -C output

	# use gtk-update-icon-cache from gtk+2.0 for now. The icon cache is forward
	# compatible so this is fine
	rm -f "$pkgdir"/usr/bin/gtk-update-icon-cache
	rm -f "$pkgdir"/usr/share/man/man1/gtk-update-icon-cache.1
}

demo() {
	pkgdesc="$pkgdesc (demonstration application)"
	amove \
		usr/bin/gtk4-demo \
		usr/bin/gtk4-demo-application \
		usr/bin/gtk4-icon-browser \
		usr/bin/gtk4-print-editor \
		usr/bin/gtk4-widget-factory \
		usr/share/applications/org.gtk.Demo4.desktop \
		usr/share/applications/org.gtk.IconBrowser4.desktop \
		usr/share/applications/org.gtk.PrintEditor4.desktop \
		usr/share/applications/org.gtk.WidgetFactory4.desktop \
		usr/share/glib-2.0/schemas/org.gtk.Demo4.gschema.xml \
		usr/share/gtk-4.0/gtk4builder.rng \
		usr/share/icons
}
