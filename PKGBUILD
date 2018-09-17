# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Thomas Dziedzic < gostrc at gmail >

pkgbase=python-tornado
pkgname=('python-tornado' 'python2-tornado')
pkgver=5.1.1
pkgrel=1
pkgdesc='open source version of the scalable, non-blocking web server and tools'
arch=('x86_64')
url='http://www.tornadoweb.org/'
license=('Apache')
makedepends=('python-setuptools' 'python2-setuptools' 'python2-futures' 'python2-singledispatch'
             'python2-backports-abc')
checkdepends=('python-pycurl' 'python2-pycurl' 'python-mock' 'python2-mock' 'python-twisted'
              'python2-twisted'  'python2-trollius' 'python2-monotonic')
source=("$pkgbase-$pkgver.tar.gz::https://github.com/tornadoweb/tornado/archive/v$pkgver.tar.gz")
sha512sums=('b35fa0687ede6e672c35ca82fcc01dbb2ae9d8a7f2e4a65b6e07aa5a8eca861a22d775101172b7e33564951d193e5f11887021a3d762bee88668b9ac0069ab46')

prepare() {
  cd "$srcdir"
  cp -a tornado-$pkgver{,-py2}

  # python -> python2 rename
  find tornado-$pkgver-py2 -name '*py' -exec sed -e 's_#!/usr/bin/env python_&2_' -i {} \;

  export TORNADO_EXTENSION=1
}

build() {
  cd tornado-$pkgver
  python setup.py build

  cd ../tornado-$pkgver-py2
  python2 setup.py build
}

check() {
  # As of 4.5.3, ignoring test failures about resolving "localhost"
  (
    cd tornado-$pkgver
    python setup.py install --root="$PWD/tmp_install" --optimize=1
    export PYTHONPATH="$PWD/tmp_install/usr/lib/python3.7/site-packages:$PYTHONPATH"
    cd tmp_install
    python -m tornado.test.runtests
    python -m tornado.test.runtests --ioloop=tornado.platform.select.SelectIOLoop
    python -m tornado.test.runtests --httpclient=tornado.curl_httpclient.CurlAsyncHTTPClient
    python -m tornado.test.runtests --ioloop_time_monotonic
    python -m tornado.test.runtests --ioloop=tornado.platform.twisted.TwistedIOLoop
    python -m tornado.test.runtests --ioloop=tornado.platform.asyncio.AsyncIOLoop
    python -m tornado.test.runtests --resolver=tornado.netutil.ThreadedResolver
  ) || warning "Tests failed"

  (
    cd tornado-$pkgver-py2
    python2 setup.py install --root="$PWD/tmp_install" --optimize=1
    export PYTHONPATH="$PWD/tmp_install/usr/lib/python2.7/site-packages:$PYTHONPATH"
    cd tmp_install
    python2 -m tornado.test.runtests
    python2 -m tornado.test.runtests --ioloop=tornado.platform.select.SelectIOLoop
    python2 -m tornado.test.runtests --httpclient=tornado.curl_httpclient.CurlAsyncHTTPClient
    python2 -m tornado.test.runtests --ioloop_time_monotonic
    python2 -m tornado.test.runtests --ioloop=tornado.platform.twisted.TwistedIOLoop
    python2 -m tornado.test.runtests --ioloop=tornado.platform.asyncio.AsyncIOLoop
    python2 -m tornado.test.runtests --resolver=tornado.netutil.ThreadedResolver
  ) || warning "Tests failed"
}

package_python-tornado() {
  depends=('python')
  optdepends=('python-pycurl: for tornado.curl_httpclient'
              'python-twisted: for tornado.platform.twisted')
              # 'python-pycares: an alternative non-blocking DNS resolver'

  cd tornado-$pkgver
  python setup.py install --root="$pkgdir" --optimize=1
}

package_python2-tornado() {
  depends=('python2-futures' 'python2-singledispatch' 'python2-backports-abc')
  optdepends=('python2-monotonic: enable support for a monotonic clock'
              'python2-pycurl: for tornado.curl_httpclient'
              'python2-twisted: for tornado.platform.twisted')
              # 'python2-pycares: an alternative non-blocking DNS resolver'

  cd tornado-$pkgver-py2
  python2 setup.py install --root="$pkgdir" --optimize=1
}
