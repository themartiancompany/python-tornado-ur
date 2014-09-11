# Maintainer: Felix Yan <felixonmars@gmail.com>
# Contributor: Thomas Dziedzic < gostrc at gmail >

pkgname=('python-tornado' 'python2-tornado')
pkgver=4.0.2
pkgrel=1
pkgdesc='open source version of the scalable, non-blocking web server and tools'
arch=('i686' 'x86_64')
url='http://www.tornadoweb.org/'
license=('Apache')
makedepends=('python-setuptools' 'python2-setuptools' 'python2-backports.ssl_match_hostname' 'git')
checkdepends=('python-pycurl' 'python2-pycurl' 'python-mock' 'python2-mock' 'python-twisted' 'python2-twisted')
source=("git+https://github.com/facebook/tornado.git#tag=v$pkgver"
        use_system_ca_certificates.patch)
sha512sums=('SKIP'
            '6e50e9ecf361d54d9f67e1f12185cf58863ad0eae72fbe7cc24e8eaf94874255009a030249bb51adf06e98c7ed0b17d8c6d9ee65190ebc341d6857c0efbc7840')

prepare() {
  cd tornado
  patch -p1 -i ../use_system_ca_certificates.patch

  cd "$srcdir"
  cp -a tornado{,-py2}

  # python -> python2 rename
  find tornado-py2 -name '*py' -exec sed -e 's_#!/usr/bin/env python_&2_' -i {} \;
}

build() {
  cd tornado
  python setup.py build

  cd ../tornado-py2
  python2 setup.py build
}

check() {
  cd tornado
  python -m tornado.test.runtests || warning "Tests failed"
  python -m tornado.test.runtests --ioloop=tornado.platform.asyncio.AsyncIOLoop || warning "Tests with AsyncIO failed"
  python -m tornado.test.runtests --ioloop=tornado.platform.select.SelectIOLoop || warning "Tests with SelectIO failed"
  python -m tornado.test.runtests --ioloop=tornado.platform.twisted.TwistedIOLoop || warning "Tests with TwistedIO failed"

  cd ../tornado-py2
  python2 -m tornado.test.runtests || warning "Tests failed"
  python2 -m tornado.test.runtests --ioloop=tornado.platform.twisted.TwistedIOLoop || warning "Tests with TwistedIO failed"
  python2 -m tornado.test.runtests --ioloop=tornado.platform.select.SelectIOLoop || warning "Tests with SelectIO failed"
}

package_python-tornado() {
  cd tornado
  python setup.py install --root="${pkgdir}" --optimize=1
}

package_python2-tornado() {
  depends=('python2-backports.ssl_match_hostname')

  cd tornado-py2
  python2 setup.py install --root="${pkgdir}" --optimize=1
}

