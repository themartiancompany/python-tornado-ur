# Maintainer: Felix Yan <felixonmars@gmail.com>
# Contributor: Thomas Dziedzic < gostrc at gmail >

pkgname=('python-tornado' 'python2-tornado')
pkgver=3.0.2
pkgrel=1
pkgdesc='open source version of the scalable, non-blocking web server and tools'
arch=('any')
url='http://www.tornadoweb.org/'
license=('Apache')
makedepends=('python-distribute' 'python2-distribute')
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
sha512sums=('9bf091f1e67a841b831fdbb84496580f8374e2ae59e8e06c7d955dc26dfe2d358e51791a9e75058a544d613d5670c1ec90151e6b641262a7833a9eed8112ea92')
