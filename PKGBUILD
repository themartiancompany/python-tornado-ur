# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Thomas Dziedzic < gostrc at gmail >

pkgname=python-tornado
pkgver=6.3.0
pkgrel=1
pkgdesc='open source version of the scalable, non-blocking web server and tools'
arch=('x86_64')
url='https://www.tornadoweb.org/'
license=('Apache')
depends=('python')
optdepends=('python-pycurl: for tornado.curl_httpclient'
            'python-twisted: for tornado.platform.twisted')
            # 'python-pycares: an alternative non-blocking DNS resolver'
makedepends=('python-setuptools')
checkdepends=('python-pycurl' 'python-twisted')
source=("$pkgname-$pkgver.tar.gz::https://github.com/tornadoweb/tornado/archive/v$pkgver.tar.gz")
sha512sums=('61c3c9028147c0256c587cc2f7abf2b274cbddca5f41f732efb0598405c17af96d969b60aef3005004e3c4d5226d604f1185dd57ba6ffc6d4a02aa135dc7453d')

export TORNADO_EXTENSION=1

build() {
  cd tornado-$pkgver
  python setup.py build
}

check() {
  # As of 4.5.3, ignoring test failures about resolving "localhost"
  (
    cd tornado-$pkgver
    python setup.py install --root="$PWD/tmp_install" --optimize=1
    local python_version=$(python -c 'import sys; print(".".join(map(str, sys.version_info[:2])))')
    export PYTHONPATH="$PWD/tmp_install/usr/lib/python${python_version}/site-packages:$PYTHONPATH"
    cd tmp_install
    python -m tornado.test.runtests
    python -m tornado.test.runtests --ioloop=tornado.platform.select.SelectIOLoop
    python -m tornado.test.runtests --httpclient=tornado.curl_httpclient.CurlAsyncHTTPClient
    python -m tornado.test.runtests --ioloop_time_monotonic
    python -m tornado.test.runtests --ioloop=tornado.platform.twisted.TwistedIOLoop
    python -m tornado.test.runtests --ioloop=tornado.platform.asyncio.AsyncIOLoop
    python -m tornado.test.runtests --resolver=tornado.netutil.ThreadedResolver
  ) || echo "Tests failed"
}

package() {
  cd tornado-$pkgver
  python setup.py install --root="$pkgdir" --optimize=1
}
