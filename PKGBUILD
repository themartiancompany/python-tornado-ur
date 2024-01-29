# SPDX-License-Identifier: AGPL-3.0
#
# Contributor: Felix Yan <felixonmars@archlinux.org>
# Contributor: Thomas Dziedzic < gostrc at gmail >
# Maintainer:  Pellegrino Prevete <cGVsbGVncmlub3ByZXZldGVAZ21haWwuY29tCg== | base -d>
# Maintainer:  Truocolo <truocolo@aol.com>

_py="python"
_pkg='tornado'
pkgname="${_py}-${_pkg}"
pkgver=6.3.2
pkgrel=1
_pkgdesc=(
  'open source version of the scalable,'
  'non-blocking web server and tools'
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'x86_64'
  'arm'
  'aarch64'
  'powerpc'
  'i686'
)
url="https://www.${_pkg}web.org"
license=(
  'Apache'
)
depends=(
  "${_py}"
)
optdepends=(
  "${_py}-pycurl: for tornado.curl_httpclient"
  "${_py}-twisted: for tornado.platform.twisted"
  # "${_py}-pycares: an alternative non-blocking DNS resolver"
)
makedepends=(
  "${_py}-setuptools"
)
checkdepends=(
  "${_py}-pycurl"
  "${_py}-twisted"
)
_url="https://github.com/${_pkg}web/${_pkg}"
source=(
  "${pkgname}-${pkgver}.tar.gz::${_url}/archive/v${pkgver}.tar.gz"
)
sha512sums=(
  'dc0ad9b4c0b5597970fd43a577bb9a0883523125cf4e9780f9338431aab1014cb6fc0dda4f3deb3050df657b5acf277cc146ec2195b91154299109ff07482a5c'
)

export \
 TORNADO_EXTENSION=1

build() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    setup.py \
      build
}

check() {
  # As of 4.5.3, ignoring test
  # failures about resolving "localhost"
  (
    cd "${_pkg}-${pkgver}"
    "${_py}" \
      setup.py \
        install \
	--root="$PWD/tmp_install" \
	--optimize=1
    local \
      python_version=$( \
        "${_py}" \
	  -c \
	    'import sys; print(".".join(map(str, sys.version_info[:2])))')
    export \
      PYTHONPATH="$PWD/tmp_install/usr/lib/python${python_version}/site-packages:$PYTHONPATH"
    cd \
      tmp_install
    "${_py}" \
      -m "${_pkg}.test.runtests"
    "${_py}" \
      -m "${_py}.test.runtests" \
        --ioloop="${_pkg}.platform.select.SelectIOLoop"
    "${_py}" \
      -m "${_py}.test.runtests" \
        --httpclient="${_pkg}.curl_httpclient.CurlAsyncHTTPClient"
    "${_py}" \
      -m "${_py}.test.runtests" \
      --ioloop_time_monotonic
    "${_py}" \
      -m "${_py}.test.runtests" \
      --ioloop="${_py}.platform.twisted.TwistedIOLoop"
    "${_py}" \
      -m "${_py}.test.runtests" \
      --ioloop="${_pkg}.platform.asyncio.AsyncIOLoop"
    "${_py}" \
      -m "${_py}.test.runtests" \
      --resolver="${_pkg}.netutil.ThreadedResolver"
  ) || \
    echo \
      "Tests failed"
}

package() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    setup.py \
      install \
        --root="${pkgdir}" \
	--optimize=1
}

# vim:set sw=2 sts=-1 et:
