# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Thomas Dziedzic < gostrc at gmail >

pkgname=('python-tornado' 'python2-tornado')
pkgver=4.1.0
pkgrel=1
pkgdesc='open source version of the scalable, non-blocking web server and tools'
arch=('i686' 'x86_64')
url='http://www.tornadoweb.org/'
license=('Apache')
makedepends=('python-setuptools' 'python2-setuptools' 'git')
checkdepends=('python-pycurl' 'python2-pycurl' 'python-mock' 'python2-mock' 'python-twisted' 'python2-twisted' 'python2-futures' 'python2-singledispatch')
source=("git+https://github.com/facebook/tornado.git#tag=v$pkgver"
        0001-use_system_ca_certificates.patch
        0002-get-rid-of-backports-ssl-match-hostname.patch)
sha512sums=('SKIP'
            '6e50e9ecf361d54d9f67e1f12185cf58863ad0eae72fbe7cc24e8eaf94874255009a030249bb51adf06e98c7ed0b17d8c6d9ee65190ebc341d6857c0efbc7840'
            '798f1c5f659138aa4d775edde7c962ec6410671f528b7ec44ca12ac342ddf9ec51d998c676b9025292a58c2140ba8492fcc76759b63adaf08320f96b11bcbfea')

prepare() {
  cd tornado
  patch -p1 -i ../0001-use_system_ca_certificates.patch
  patch -p1 -i ../0002-get-rid-of-backports-ssl-match-hostname.patch

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
  (
    cd tornado
    # TODO: exporting PYTHONPATH didn't fix the tornado.speedups not found problem...
    export PYTHONPATH="$(pwd)/build/lib.linux-$CARCH-3.4:$PYTHONPATH"
    python -m tornado.test.runtests
    python -m tornado.test.runtests --httpclient=tornado.curl_httpclient.CurlAsyncHTTPClient
    # python -m tornado.test.runtests --resolver=tornado.platform.caresresolver.CaresResolver  # pycares not in the repos
    python -m tornado.test.runtests --resolver=tornado.netutil.ThreadedResolver
    python -m tornado.test.runtests --ioloop=tornado.platform.asyncio.AsyncIOLoop
    python -m tornado.test.runtests --ioloop=tornado.platform.select.SelectIOLoop
    python -m tornado.test.runtests --ioloop=tornado.platform.twisted.TwistedIOLoop
    python -m tornado.test.runtests --ioloop=tornado.test.twisted_test.LayeredTwistedIOLoop --resolver=tornado.platform.twisted.TwistedResolver
  ) || warning "Python 3 tests failed"

  (
    cd tornado-py2
    export PYTHONPATH="$(pwd)/build/lib.linux-$CARCH-2.7:$PYTHONPATH"
    python2 -m tornado.test.runtests
    python2 -m tornado.test.runtests --httpclient=tornado.curl_httpclient.CurlAsyncHTTPClient
    # python2 -m tornado.test.runtests --resolver=tornado.platform.caresresolver.CaresResolver  # pycares not in the repos
    python2 -m tornado.test.runtests --resolver=tornado.netutil.ThreadedResolver
    python2 -m tornado.test.runtests --ioloop=tornado.platform.select.SelectIOLoop
    python2 -m tornado.test.runtests --ioloop=tornado.platform.twisted.TwistedIOLoop
    python2 -m tornado.test.runtests --ioloop=tornado.test.twisted_test.LayeredTwistedIOLoop --resolver=tornado.platform.twisted.TwistedResolver
  ) || warning "Python 2 tests failed"
}

package_python-tornado() {
  depends=('python')

  cd tornado
  python setup.py install --root="${pkgdir}" --optimize=1
}

package_python2-tornado() {
  depends=('python2>=2.7.9')

  cd tornado-py2
  python2 setup.py install --root="${pkgdir}" --optimize=1
}

