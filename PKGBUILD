# Contrubutor: Alexander Kreuzer <alex@freesources.org>
# based on hlint package from
# Arch Haskell Team <arch-haskell@haskell.org>
pkgname=hlint-darcs
pkgrel=1
pkgver=20111202
pkgdesc="Source code suggestions (darcs version)"
url="http://community.haskell.org/~ndm/hlint/"
license=('GPL')
arch=('i686' 'x86_64')
makedepends=('darcs')
depends=('ghc' 'haskell-cpphs' 'haskell-haskell-src-exts>=1.11' 'haskell-hscolour' 'haskell-mtl' 'haskell-uniplate')
provides=('hlint=1.8.20' 'haskell-hlint=1.8.20')
options=('strip')
install=hlint.install
source=()
md5sums=()

_darcsmod="hlint"
_darcstrunk="http://community.haskell.org/~ndm/darcs"

build() {
    cd ${srcdir}

    # Erasing previous build
    msg "Checking for previous build"
    # get the sources
    if [[ -d $_darcsmod/_darcs ]]
    then
        msg "Retrieving missing patches"
        cd $_darcsmod
        darcs pull -a $_darcstrunk/$_darcsmod
    else
        msg "Retrieving complete sources"
        darcs get --partial --set-scripts-executable $_darcstrunk/$_darcsmod
        cd $_darcsmod
    fi

    runhaskell Setup configure --prefix=/usr --docdir=/usr/share/doc/${pkgname}
    runhaskell Setup build                  
    runhaskell Setup haddock
    runhaskell Setup register   --gen-script
    runhaskell Setup unregister --gen-script
}

package() {
  cd $srcdir/$_darcsmod  
  install -D -m744 register.sh   ${pkgdir}/usr/share/haskell/$pkgname/register.sh
  install    -m744 unregister.sh ${pkgdir}/usr/share/haskell/$pkgname/unregister.sh
  install -d -m755 $pkgdir/usr/share/doc/ghc/html/libraries
  ln -s /usr/share/doc/${pkgname}/html ${pkgdir}/usr/share/doc/ghc/html/libraries/hlint
  runhaskell Setup copy --destdir=${pkgdir}
}
