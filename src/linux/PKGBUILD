pkgname=smartos-vmtools-git
pkgver=20130109
pkgrel=1
pkgdesc="Joynet SmartOS vmtools"
arch=('any')
url="http://github.com/joyent/smartos-vmtools"
license=('Joynet')
depends=('pacman')
makedepends=('git' 'perl')
conflicts=('smartos-vmtools')
provides=('smartos-vmtools')

# because the sources are not static, skip checksums
md5sums=('SKIP')
_gitroot="git://github.com/nvandal/smartos-vmtools.git"
_gitname="smartos-vmtools"

build() {
 cd "$srcdir"
 msg "Connecting to GIT server..."
 if [ -d $_gitname ] ; then
   cd $_gitname && git pull origin
   msg "The local files are updated."
 else
   git clone --depth=1 $_gitroot $_gitname
 fi
 msg "GIT checkout done or server timeout"
 rm -rf "$srcdir/$_gitname-build" 
 mkdir "$srcdir/$_gitname-build"
 cd "$srcdir/$_gitname" && ls -A | grep -v .git | xargs -d '\n' cp -r -t ../$_gitname-build # do not copy over the .git folder
 cd "$srcdir/$_gitname-build"
}

package() {
  cd "$srcdir/$_gitname-build/src/linux/lib/smartdc"
  for file in *
  do
     #Correct the paths in the script files
     sed 's|/lib/smartdc|/usr/lib/smartdc|' $file > $file.tmp
     mv  $file.tmp $file
     install -D $file $pkgdir/usr/lib/smartdc/$file 
  done
  
  cd "../systemd/system"
  for file in *.service
  do
     #Correct the paths in the script files
     sed 's|/lib/smartdc|/usr/lib/smartdc|' $file > $file.tmp
     mv  $file.tmp $file
     install -D $file $pkgdir/usr/lib/systemd/system/$file
  done
}

