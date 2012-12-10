# Maintainer: Felix Yan <felixonmars@gmail.com>
# Contributor: Thomas Dziedzic < gostrc at gmail >

pkgname=('python-tornado' 'python2-tornado')
pkgver=2.4.1
pkgrel=1
pkgdesc='open source version of the scalable, non-blocking web server and tools'
arch=('any')
url='http://www.tornadoweb.org/'
license=('Apache')
makedepends=('python-distribute' 'python2-distribute')
source=("https://github.com/downloads/facebook/tornado/tornado-${pkgver}.tar.gz")

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
md5sums=('9b7146cbe7cce015e35856b592707b9b')
