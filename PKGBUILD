# Maintainer: Felix Yan <felixonmars@gmail.com>
# Contributor: Thomas Dziedzic < gostrc at gmail >

pkgname=('python-tornado' 'python2-tornado')
pkgver=3.1.1
pkgrel=1
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

sha512sums=('57decd9d36aa383dcea20f380fe5b019a228eecb040af3cb6550784605739268dbc1b48378bba2cbce3e7b26d09745bada0f82b2631577a600a82c236b17aa35')
