# Maintainer: Felix Yan <felixonmars@gmail.com>
# Contributor: Thomas Dziedzic < gostrc at gmail >

pkgname=('python-tornado' 'python2-tornado')
pkgver=3.1.0
pkgrel=2
pkgdesc='open source version of the scalable, non-blocking web server and tools'
arch=('any')
url='http://www.tornadoweb.org/'
license=('Apache')
makedepends=('python-setuptools' 'python2-setuptools')
source=("https://github.com/facebook/tornado/archive/v${pkgver}.tar.gz")

build() {
  cd "$srcdir"
  cp -r tornado-${pkgver} python2-tornado-${pkgver}

  cd tornado-${pkgver}

  python setup.py build

  cd ../python2-tornado-${pkgver}

  # python -> python2 rename
  find -name '*py' -exec sed -e 's_#!/usr/bin/env python_&2_' -i {} \;

  python2 setup.py build
}

package_python-tornado() {
  depends=('python')

  cd "$srcdir/tornado-${pkgver}"

  python setup.py install --root="${pkgdir}" --optimize=1
}

package_python2-tornado() {
  depends=('python2')

  cd "$srcdir/python2-tornado-${pkgver}"

  python2 setup.py install --root="${pkgdir}" --optimize=1
}

sha512sums=('841127ab2f60e41d2f02a50fc5a8187a6907a745de81bac55ec942e69c1985a10b2d709261e920b26b31275094196ce943c01de71a4c0bb3015e48cc136652fd')
