# Maintainer: Raymond Smith <raymondbarrettsmith at gmail dot com>
pkgname=goldencheetah
pkgver=3.1
pkgrel=1
pkgdesc="A set of analysis tools for cycling performance"
arch=('x86_64' 'i686')
url="http://goldencheetah.org/"
license=('GPL2')
depends=('qt4' 'qtwebkit' 'libical' 'vlc')
#depends=('qt4' 'qtwebkit' 'libical' 'vlc' 'qwtplot3d')
makedepends=('git' 'clucene' 'desktop-file-utils')
optdepends=('qwtplot3d: 3D plotting')
source=(git://github.com/GoldenCheetah/GoldenCheetah.git#tag=v${pkgver}
        goldencheetah.desktop)
sha256sums=('SKIP'
            '8b22984b3f058a77f207aecfeb8738b87ec389f1c3a4142f72c0024c53c8ea61')
_gitname=GoldenCheetah

# NOTE!!
# Because of this issue with qwt/qmake (http://sourceforge.net/p/qwt/bugs/150/),
# this package cannot be build with qwt installed. However, once built, qwt can
# be installed.
# See comments on GoldenCheetah mailing list at:
# https://groups.google.com/forum/#!topic/golden-cheetah-users/nsnonUPwHQE
# So, to build:

# {as root}
# pacman -R qwt

# {not as root}
# cd {directory with goldencheetah PKGBUILD and desktop file}
# makepkg

# {as root}
# pacman -U goldencheetah
# pacman -S qwt

# And of course, with all the qwt steps, this would also involve removing
# and reinstalling those packages that depend on qwt.

build() {
  cd "${srcdir}/${_gitname}"
  cp qwt/qwtconfig.pri.in qwt/qwtconfig.pri
  cp src/gcconfig.pri.in src/gcconfig.pri
  # change executable name
  sed -i "s|#\(APP_NAME =.*\)|\1${pkgname}|" src/gcconfig.pri
  # uncomment QMAKE_LRELEASE and change name of binary
  sed -i "s|#\(QMAKE_LRELEASE.*\)|\1-qt4|" src/gcconfig.pri
  # uncomment -O3 optimization flag
  sed -i "s|#\(QMAKE_CXXFLAGS += -O3\)|\1|" src/gcconfig.pri
  # use default paths for flex and bison
  sed -i "27,33 s|^#||" src/gcconfig.pri
  # Use libical for diary view (calendars)
  sed -i "s|#\(ICAL_INSTALL =.*\)|\1 /usr|" src/gcconfig.pri
  sed -i "s|#\(ICAL_LIBS *=.*\)|\1 -lical|" src/gcconfig.pri
  # Use clucene for search function
  sed -i "s|#\(CLUCENE_\)|\1|" src/gcconfig.pri
  sed -i "s|\(CLUCENE_INCLUDE =\).*|\1 /usr/lib|" src/gcconfig.pri
  # Use vlc for video playback
  sed -i "s|#\(VLC_INSTALL =\).*|\1 /usr|" src/gcconfig.pri
  sed -i "s|\(CLUCENE_INCLUDE =\).*|\1 /usr/include/vlc|" src/gcconfig.pri
  # Uncomment if you have qwtplot3d installed and want 3D plotting
#  sed -i "s|#\(QWT3D_INSTALL =.*\)|\1 /usr|" src/gcconfig.pri
#  sed -i "s|#\(QWT3D_INCLUDE =.*\)|\1 /usr/include/qwtplot3d|" src/gcconfig.pri
#  sed -i "s|#\(QWT3D_LIBS    =.*\)|\1 /usr/lib/libqwtplot3d.so|" src/gcconfig.pri
  qmake-qt4
  make
}

package() {
  install -Dm755 ${srcdir}/${_gitname}/src/${pkgname} \
    ${pkgdir}/usr/bin/${pkgname}
  desktop-file-install --dir="${pkgdir}/usr/share/applications" \
    "${srcdir}/${pkgname}.desktop"
}

# vim:set ts=2 sw=2 et tw=0:
