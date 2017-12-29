# Contributor: Sasha Gerrand <alpine-pkgs@sgerrand.com>
# Maintainer: Sasha Gerrand <alpine-pkgs@sgerrand.com>
pkgname=java-openjfx
_java_ver=8
_jdk_update=151
_jdk_build=12
_hgtag=${_java_ver}u${_jdk_update}-b${_jdk_build}
pkgver=${_java_ver}.${_jdk_update}.${_jdk_build}
pkgrel=0
pkgdesc="Java OpenJFX 8 client application platform (open-source implementation of JavaFX)"
url="https://wiki.openjdk.java.net/display/OpenJFX/Main"
arch="all"
license="GPL"
depends="openjdk8-jre-base ffmpeg gstreamer libxtst qt5-qtbase webkit2gtk"
makedepends="openjdk8 bash ffmpeg-dev gtk+2.0-dev libc-dev libjpeg-turbo-dev libxtst-dev ncurses pango-dev python2 unzip webkit2gtk-dev"
install=""
subpackages="$pkgname-dev $pkgname-doc"
source="http://hg.openjdk.java.net/openjfx/8u-dev/rt/archive/${_hgtag}.tar.bz2
        https://services.gradle.org/distributions/gradle-1.8-bin.zip
        01-gradle-set-java-versions.patch
        02-jfxpanel.patch
        03-musl-compatibility.patch
        04-fix-gcc-sentinel-warnings.patch
        17-gcc-compatibility.patch::https://anonscm.debian.org/cgit/pkg-java/openjfx.git/plain/debian/patches/17-gcc-compatibility.patch"
builddir="$srcdir/rt-${_hgtag}"

prepare() {
	cd "$builddir"

	default_prepare

	cat > gradle.properties << EOF
COMPILE_WEBKIT = false
COMPILE_MEDIA = false
BUILD_JAVADOC = true
BUILD_SRC_ZIP = true
JDK_HOME=/usr/lib/jvm/java-1.8-openjdk
EOF

	find -name '*.class' -delete
	find -name '*.jar' -delete

	rm -rf modules/media/src/main/native/gstreamer/3rd_party/glib
	rm -rf modules/media/src/main/native/gstreamer/gstreamer-lite
	rm -rf modules/graphics/src/main/native-iio/libjpeg*
}

build() {
	cd "$builddir"

	export GRADLE_USER_HOME="$srcdir"/gradle_home
	mkdir -p "$GRADLE_USER_HOME"

	"$srcdir"/gradle-1.8/bin/gradle --no-daemon
}

package() {
	cd "$builddir"

	local _builddir="${srcdir}/rt-${_hgtag}/build"
	local _sdkdir="${_builddir}/sdk"
	local _openjdk8dir=/usr/lib/jvm/java-1.8-openjdk

	install -d "${pkgdir}${_openjdk8dir}/jre/lib/${_CARCH}"
	install -m755 "${_sdkdir}/rt/lib/${_CARCH}"/*.* "${pkgdir}${_openjdk8dir}/jre/lib/${_CARCH}"

	install -d "${pkgdir}${_openjdk8dir}/jre/lib/ext"
	install -m644 "${_sdkdir}/rt/lib/ext"/*.* "${pkgdir}${_openjdk8dir}/jre/lib/ext"
	install -m644 "${_sdkdir}/rt/lib"/*.* "${pkgdir}${_openjdk8dir}/jre/lib"

	install -d "${pkgdir}${_openjdk8dir}/lib"
	install -m644 "${_sdkdir}/lib"/*.* "${pkgdir}${_openjdk8dir}/lib"

	install -d "${pkgdir}${_openjdk8dir}/bin"
	install -m755 "${_sdkdir}/bin"/* "${pkgdir}${_openjdk8dir}/bin"

	install -m644 -D "${_sdkdir}/man/man1/javapackager.1" "${pkgdir}/usr/share/man/man1/javapackager.1"
}

dev() {
	default_dev

	mkdir -p "$subpkgdir"
}

doc() {
	default_doc

	mkdir -p "$subpkgdir"
}

sha512sums="e9c3c403528736b5a03882f72abdf7965ba9deea59ef4ef31d81d724d0e5d3dc36d9405fceb95d309b6e36ce4e92dccad9bc6dac2c40463d156d93b7fed3d44b  8u151-b12.tar.bz2
2692f36626576a511e688ff42fc3381f8ecdf4fdf537939ab59de6c807eb17f4b4df722c4912e373ad3d9fe95be8516841ccb78a00111b5a7d38b848e632337b  gradle-1.8-bin.zip
6a430d751e6afa3773b3d23e9e5db878291fea40b9c03b4438404416171d758fcbf05d73d6c89645b3039217349e3fffc02477e42f9dd5e6f536f9aaad95f78f  01-gradle-set-java-versions.patch
cafd180d3058ed1f871a70e11625a9b4e92850a80f106fa8a3e2fe2d88b94c01525b45a6d97b69e38b53bcd499c9a6573879326308d45f5abbc2b79e0bfb4312  02-jfxpanel.patch
fd92bd256d6efedc45dd4abaa00967483688575215a11227e3eff60ac36979b50915f59953c662a66dafcce704a94d41694b3ad257add4cce182e29e0b817911  03-musl-compatibility.patch
64afb23bb14595654b21baa20143c85a708e748b955a86c2a456853ee791d79f66ff8a266b1ae338ef15199ec0b49e8bd31a651439953eb183f5d51f3e6a281d  04-fix-gcc-sentinel-warnings.patch
17c57781a9e1dd623206c019dd53ccd077b4ea6d94eb90fcb08ef136c2ad84094061928f774dcce27d06d3e07c538e77cd4333ad0186feca596026a46b3845ef  17-gcc-compatibility.patch"
