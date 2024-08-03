# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>

_pep517=false
_py="python"
_pkg=yarl
pkgname="${_py}-${_pkg}"
pkgver=1.9.4
pkgrel=2
pkgdesc='Yet another URL library'
url='https://github.com'
url='aio-libs'
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
  'glibc'
  "${_py}"
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
  'e31a36539166034f3b231e1f9fc47b7d0d1aea0424b6054e1858eefa9f290350ee8b1c74bb90a120d6b9f3c13fe7b675d6e0676272b3222b788d479ae9fd3ff5'
)
b2sums=(
  'c0022b32b41c1125d788c656883b3552314b138601fef72cc55ce90fc9986f44912395977ba6ac27d344c0a3593172265fb664eb6a696de9787a2474f61d14ce'
)

prepare() {
  cd \
    "${_pkgname}-${pkgver}"
  sed \
    's| .install-cython ||g' \
    -i \
    Makefile
}

build() {
  cd \
    "${_pkgname}-${pkgver}"
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
    "${_pkgname}-${pkgver}"
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
    ${_pkgname}-${pkgver}
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
