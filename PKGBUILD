# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Jelle van der Waa <jelle@archlinux.org>
# Contributor: Eli Schwartz <eschwartz@archlinux.org>
# Contributor: Daniel J Griffiths <ghost1227@archlinux.us>
# Contributor: Erol V. Aktay <e.aktay@gmail.com>

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
_pkg=html5lib
_Pkg="${_pkg}-python"
pkgname="${_py}-${_pkg}"
pkgver=1.1
pkgrel=14
arch=(
  'any'
)
_http="https://github.com"
_ns="${_pkg}"
url="${_http}/${_ns}"
license=(
  'MIT'
)
pkgdesc="A Python HTML parser/tokenizer based on the WHATWG HTML5 spec"
_deps=(
  'six'
  'webencodings'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_deps[@]/#/python-}"
)
makedepends=(
  "${_py}-setuptools"
  "${_deps[@]/#/python-}"
)
_checkdeps=(
  'pytest'
  'pytest-expect'
)
checkdepends=(
  "${_checkdeps[@]/#/python-}"
  "${_py}-lxml"
)
optdepends=(
  "${_py}-lxml: lxml treebuilder"
  "${_py}-genshi: genshi treewalker"
)
_url="${url}/${_Pkg}"
_test_commit="71eebd59772d1d39aced0c0582ae9c09acf3ce6e"
_patch_commit="2c19b9899ab3a3e8bd0ca35e5d78544334204169"
_Patch_commit="51cd4b9d331da8b167d0d0a77deb62357b42a1b7"
_PAtch_commit="5ec6e5e614679ee938545704fad958a3491c678b"
_tests_url="${url}/${_pkg}-tests"
source=(
  "${_pkg}-${pkgver}.tar.gz::${_url}/archive/${pkgver}.tar.gz"
  "pytest6-compat.patch::${_url}/commit/${_patch_commit}.patch"
  "${_tests_url}/archive/${_test_commit}.tar.gz"
  "html5lib_pytest74.patch::${_url}/pull/570/commits/${_Patch_commit}.patch"
  "html5lib-mock-stdlib.patch::${_url}/commit/${_PAtch_commit}.patch"
)
sha256sums=(
  '66e9e24a53c10c27abb6be8a3cf2cf55824c6ea1cef8570a633cb223ec46e894'
  '6f37b33667e35ea138279be9842f012792821b1497b81ae4b108a74180cfe249'
  'c866f5e4ae9ef34313e2b61fcb74849b6f8aef970bf8249956b6a5d142197087'
  '12921399944e191a01a93c26f684b780bc17182b8b642f39591446fd52ae5a34'
  '084aeb6fe430a82e482bae01e7f962308f77107d97c42316e512efb7045f7030'
)

prepare() {
  cd \
    "${_Pkg}-${pkgver}"
  patch \
    -p1 \
    -i "../pytest6-compat.patch"
  patch \
    -p1 \
    -i \
    "../html5lib_pytest74.patch"
  patch \
    -p1 \
    -i \
    "../html5lib-mock-stdlib.patch"
  rmdir \
    "html5lib/tests/testdata"
  ln \
    -sfT \
    "../../../html5lib-tests-${_test_commit}" \
    "html5lib/tests/testdata"
}

build() {
  cd \
    "${_Pkg}-${pkgver}"
  "${_py}" \
    setup.py \
      build
}

check() {
  cd \
    "${_Pkg}-${pkgver}"
  pytest
}

package() {
  cd \
    "${_Pkg}-${pkgver}"
  "${_py}" \
    setup.py \
      install \
        --root="${pkgdir}" \
	--optimize=1 \
	--skip-build
  install \
    -Dm644 \
    LICENSE \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
