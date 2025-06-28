# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer:
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Contributor:
#   Felix Yan
#     <felixonmars@archlinux.org>
# Contributor:
#   Thomas Dziedzic < gostrc at gmail >

_py="python"
_pkg='tornado'
pkgname="${_py}-${_pkg}"
pkgver=6.3.2
pkgrel=1
_pkgdesc=(
  'Open source version of the scalable,'
  'non-blocking web server and tools.'
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
