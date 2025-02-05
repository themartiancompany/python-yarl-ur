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

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>

_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="glibc"
fi
_pep517='true'
_py="python"
_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=yarl
pkgname="${_py}-${_pkg}"
pkgver=1.17.1
pkgrel=1
pkgdesc='Yet another URL library'
_http='https://github.com'
_ns='aio-libs'
url="${_http}/${_ns}/${_pkg}"
arch=(
  'x86_64'
  'arm'
  'aarch64'
  'armv7l'
  'i686'
  'pentium4'
  'powerpc'
  'mips'
)
license=(
  'Apache-2.0'
)
depends=(
  "${_libc}"
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-multidict"
  "${_py}-idna"
)
makedepends=(
  'cython'
  "${_py}-setuptools"
  "${_py}-wheel"
  "${_py}-expandvars"
)
[[ "${_pep517}" == true ]] && \
  makedepends+=(
    "${_py}-build"
    "${_py}-installer"
  )
checkdepends=(
  "${_py}-pytest"
  "${_py}-pytest-cov"
  "${_py}-pytest-xdist"
)
source=(
  "${url}/archive/v${pkgver}/${pkgname}-${pkgver}.tar.gz"
)
sha512sums=(
  '88aadff20c4ce599da5caf77ca68d1f257a3c8050a95aca9aa879acb76fb5108cc6ad4fcbe0cc0253c02b01d8c20287009eaf448927dbe057d8f29116bd06959'
)
b2sums=(
  '2e044e5879d888bc5c6cb5082342525ee84f3192f5d8b75377e38dd265510da73a669b3044e1d44cbc6bc8c4be5793e4f3afc0ed2b45a70b2068565625b321b9'
)

prepare() {
  cd \
    "${_pkg}-${pkgver}"
  sed \
    's| .install-cython ||g' \
    -i \
    Makefile
}

build() {
  cd \
    "${_pkg}-${pkgver}"
  make \
    cythonize
  export \
    LANG="en_US.UTF-8"
  if [[ "${_pep517}" == 'true' ]]; then
    "${_py}" \
      -m \
        build \
      --wheel \
      --no-isolation
  elif [[ "${_pep517}" == 'false' ]]; then
    LANG="en_US.UTF-8" \
    "${_py}" \
      setup.py \
        build \
          -O1
  fi
}

check() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      venv \
    --system-site-packages \
    test-env
  test-env/bin/python \
    -m \
      installer \
    dist/*.whl
  cd \
    tests
  ../test-env/bin/"${_py}" \
    -m pytest \
    -v
}

package() {
  cd \
    ${_pkg}-${pkgver}
  if [[ "${_pep517}" == 'false' ]]; then
    LANG="en_US.UTF-8" \
    "${_py}" \
      setup.py \
        install \
          --root="${pkgdir}" \
          -O1 \
          --skip-build
  elif [[ "${_pep517}" == 'true' ]]; then
    export \
      LANG="en_US.UTF-8"
    "${_py}" \
      -m \
        installer \
      --destdir="$pkgdir" \
      dist/*.whl
  fi
}

# vim: ts=2 sw=2 et:
