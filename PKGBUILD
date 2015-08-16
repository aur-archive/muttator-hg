# Contributor: Dave Nguyen <diendien@gmail.com>
# Inspiration: Michael Witten <mfwitten>, Gustavo Dutra <mechamo@gustavodutra.com>
# Maintainer: Fabio Zanini <zanini.fabio AT fastmail.fm>
######################################################################

pkgname=muttator-hg
license=(MIT)
pkgver=4744.f321e01078a5
pkgrel=1
pkgdesc="Make Thunderbird look and behave like Vim; built from HEAD of branch default"
arch=(any)
url=http://www.vimperator.org/muttator
depends=('thunderbird')
makedepends=(coreutils mercurial sh zip unzip)
provides=(muttator)

_hgroot=https://vimperator-labs.googlecode.com
_hgrepo=hg
_muttatorVersion="1.1"

pkgver() {
  cd "$srcdir/$_hgrepo"
  echo $(hg identify -n).$(hg identify -i)
}

build() {

    cd "$srcdir"
    msg "Connecting to server...."

    if [ -d "$_hgrepo" ] ; then
      cd "$_hgrepo" && hg pull -u
      msg "The local files are updated."
    
     else
      hg clone "$_hgroot/$_hgrepo"
      cd "$_hgrepo"

    fi
    msg "Checkout done or server timed out"

    msg "Cleaning ..."
    hg update --clean

    cd muttator
    msg "Compiling ..."
    make xpi
}

package() {

    # Get variables for the installation
    _emid=$(sed -n '/.*<em:id>\(.*\)<\/em:id>.*/{s//\1/p;q}' $srcdir/$_hgrepo/muttator/install.rdf)
    dstdir=$pkgdir/usr/lib/thunderbird/extensions/$_emid
    path_xpi="$srcdir/$_hgrepo/downloads/muttator-$_muttatorVersion.xpi"

    msg "$srcdir/$_hgrepo"
    msg "Installing ..."
    install -d "$dstdir"
    unzip -od "$dstdir" "$path_xpi"
    rm -f $dstdir/*.xpi
}

