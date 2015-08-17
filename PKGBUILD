# Contributor: Anton Shestakov <engored*ya.ru>
# Maintainer: Anton Shestakov <engored*ya.ru>

pkgname=project6014-svn
pkgver=2362
pkgrel=1
pkgdesc='Mod sequel to Star Control II: The Ur-Quan Masters (uqm), SVN version'
url='http://code.google.com/p/project6014/'
arch=('i686' 'x86_64')
license=('GPL')
depends=('libgl' 'libmikmod' 'libogg' 'libvorbis' 'sdl_image' 'zlib')
makedepends=('subversion')
conflicts=('project6014')
provides=('project6014')
source=('config.state' 'p6014' 'p6014.desktop' 'p6014.png')
md5sums=('7fc12edbbc7b30c4104b810fe94e2e26'
         '431a5a5871da04d3cb4c631ec30326b9'
         '0d7724cb53d35cc226d05af234a7bdb0'
         '83eb77bf17f7e6ad970685fd90d8c3b5')

_svntrunk='http://project6014.googlecode.com/svn/trunk/sc2'
_svnmod='project6014'

build() {
  cd "$srcdir"

  if [ -d "${_svnmod}/.svn" ]; then
    (cd "$_svnmod" && svn up -r $pkgver)
  else
    svn co "$_svntrunk" --config-dir ./ -r $pkgver $_svnmod
  fi

  msg 'SVN checkout done or server timeout'
  msg 'Starting make...'

  rm -rf "${_svnmod}-build"
  cp -r "$_svnmod" "${_svnmod}-build"
  cd "${_svnmod}-build"

  cp "$srcdir/config.state" .
  sed -i config.state -e "/INPUT_install_prefix/ s|replaceme|$pkgdir/usr|"

  echo | ./build.sh uqm config
  ./build.sh uqm
}

package() {
  cd "${srcdir}/${_svnmod}-build"
  ./build.sh uqm install

  install -Dm644 "$srcdir/p6014.desktop" "$pkgdir/usr/share/applications/p6014.desktop"
  install -Dm644 "$srcdir/p6014.png" "$pkgdir/usr/share/pixmaps/p6014.png"

  rm "$pkgdir/usr/bin/p6014"
  install -Dm755 "$srcdir/p6014" "$pkgdir/usr/bin/p6014"
}
