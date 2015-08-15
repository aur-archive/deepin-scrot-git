# Contributor: skatiger <skatiger [at] gmail [dot] com> 

pkgname=deepin-scrot-git
pkgver=20120309 # git auto added
pkgrel=1
pkgdesc="a screen snapshot tool from deepin linux"
arch=("i686" "x86_64")
url="http://www.linuxdeepin.com"
license=("LGPLv3")
depends=('python2' 'gconf' 'pygtk' 'python-xlib')
makedepends=('git')
conflicts=('deepin-scrott')
provides=('deepin-scrot')
install=deepin-scrot.install

_gitroot="git://github.com/lovesnow/deepin-scrot.git"
_gitname="deepin-scrot"

#source=('deepin-scrot.install')
build() {
  cd ${srcdir}
  msg "Connecting to GIT server...."

  if [ -d ${_gitname}/.git ] ; then
    cd ${_gitname}

    # Change remote url to anongit
    if [ -z $( git branch -v | grep anongit ) ] ; then
        git remote set-url origin ${_gitroot}
    fi
    
    git pull origin
    msg "The local files are updated."
  else
    git clone ${_gitroot} ${_gitname}
  fi

  rm -rf "$srcdir/${_gitname}-build"
  git clone "$srcdir/${_gitname}" "$srcdir/${_gitname}-build"

  msg "GIT checkout done or server timeout"
}

package() {
  cd "$srcdir/${_gitname}-build"

  sed -i -re "s/python?/python2/" src/*.py
  chmod +x src/deepinScrot.py
  #
  # locale
  #
  for pofile in po/*.po; do 
    lang=`basename $pofile .po`
    dest="$pkgdir/usr/share/deepin-scrot/locale/$lang/LC_MESSAGES"
    mkdir -p $dest
    msgfmt -o $dest/deepin-scrot.mo $pofile
  done

  #
  # install
  #
  
  #Create directories
  install -d $pkgdir/usr/share/deepin-scrot
  # src
  cp -r src $pkgdir/usr/share/deepin-scrot
  # theme
  cp -r theme $pkgdir/usr/share/deepin-scrot
  
  install -Dm644 AUTHORS "$pkgdir/usr/share/deepin-scrot"
  install -Dm644 ChangeLog "$pkgdir/usr/share/deepin-scrot"
  install -Dm644 README "$pkgdir/usr/share/deepin-scrot"
}

# vim:set ts=2 sw=2 et:
