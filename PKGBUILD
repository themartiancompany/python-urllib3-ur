# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Patrice Peterson <runiq at archlinux dot us>
# Contributor: Chris Brannon <cmbrannon79@gmail.com>
# Contributor: BorgHunter <borghunter at gmail dot com>

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
_pkg=urllib3
pkgname="${_py}-${_pkg}"
pkgver=1.26.19
pkgrel=1
pkgdesc="HTTP library with thread-safe connection pooling and file post support"
arch=(
  "any"
)
_http="https://github.com"
_ns="${_pkg}"
url="${_http}/${_ns}/${_pkg}"
license=(
  "MIT"
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
)
makedepends=(
  "${_py}-build"
  "${_py}-installer"
  "${_py}-setuptools"
  "${_py}-wheel"
)
checkdepends=(
  "${_py}-brotli"
  "${_py}-certifi"
  "${_py}-cryptography"
  "${_py}-dateutil"
  "${_py}-flaky"
  "${_py}-gcp-devrel-py-tools"
  "${_py}-idna"
  "${_py}-pyopenssl"
  "${_py}-pysocks"
  "${_py}-pytest7"
  "${_py}-pytest-freezegun"
  "${_py}-pytest-timeout"
  "${_py}-tornado"
  "${_py}-trustme"
)
optdepends=(
  "${_py}-brotli: Brotli support"
  "${_py}-certifi: security support"
  "${_py}-cryptography: security support"
  "${_py}-idna: security support"
  "${_py}-pyopenssl: security support"
  "${_py}-pysocks: SOCKS support"
)
source=(
  "${_pkg}-${pkgver}::${url}/archive/${pkgver}/${pkgname}-${pkgver}.tar.gz"
)
sha512sums=(
  '6b72012dbd85434b2441229cbdea2a94583693f904dde349780e1290d581c8a5e10fe00a287a032ed1276349d0078b530f16a133e0f164dcea18105fa3dec79a'
)

prepare() {
  # remove python-mock requirement
  find \
    "${_pkg}-${pkgver}" \
    -type \
      f \
    -iname \
      "*.py" \
    -exec \
      sed \
        's/import mock/from unittest import mock/; s/from mock/from unittest.mock/' \
	-i \
	{} \
	+
}

build() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --no-isolation
}

check() {
  local \
    pytest_options=() \
    site_packages
  _site_packages=$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")
  pytest_options=(
    -vv
    # TODO: report upstream
    --deselect \
      test/test_ssltransport.py::SingleTLSLayerTestCase::test_ssl_object_attributes
    --deselect \
      test/contrib/test_pyopenssl.py::TestSSL::test_ssl_read_timeout
    --deselect \
      test/with_dummyserver/test_socketlevel.py::TestSSL::test_ssl_read_timeout
    # Tests hang and need an adjusted
    # backported patch for pytest8
    # ${url}/commit/8c2088622059860e5411c8e37b26e402a5dda0bb
    --deselect \
      test/with_dummyserver/test_socketlevel.py::TestSSL::*
    --deselect \
      test/with_dummyserver/test_socketlevel.py::TestClients::*
    --deselect \
      test/contrib/test_pyopenssl.py::TestClientCerts::test_client_certs_two_files
    --deselect \
      test/contrib/test_pyopenssl.py::TestClientCerts::test_client_certs_one_file
    --deselect \
      test/contrib/test_pyopenssl.py::TestClientCerts::test_missing_client_certs_raises_error
    --deselect \
      test/contrib/test_pyopenssl.py::TestClientCerts::test_client_cert_with_string_password
    --deselect \
      test/contrib/test_pyopenssl.py::TestClientCerts::test_client_cert_with_bytes_password
    --deselect \
      test/contrib/test_pyopenssl.py::TestSSL::test_ssl_failure_midway_through_conn
    --deselect \
      test/contrib/test_pyopenssl.py::TestSSL::test_retry_ssl_error
    --deselect \
      test/contrib/test_pyopenssl.py::TestSSL::test_requesting_large_resources_via_ssl
    --deselect \
      test/with_dummyserver/test_proxy_poolmanager.py::TestHTTPProxyManager::test_scheme_host_case_insensitive
    --deselect \
      test/with_dummyserver/test_socketlevel.py::TestClientCerts::test_client_certs_two_files
    --deselect \
      test/with_dummyserver/test_socketlevel.py::TestClientCerts::test_client_certs_one_file
    --deselect \
      test/with_dummyserver/test_socketlevel.py::TestClientCerts::test_missing_client_certs_raises_error
    --deselect \
      test/with_dummyserver/test_socketlevel.py::TestClientCerts::test_client_cert_with_string_password
    --deselect \
      test/with_dummyserver/test_socketlevel.py::TestClientCerts::test_client_cert_with_bytes_password
    --deselect \
      test/with_dummyserver/test_socketlevel.py::TestProxyManager::test_connect_reconn
    --deselect \
      test/with_dummyserver/test_socketlevel.py::TestProxyManager::test_connect_ipv6_addr
    --deselect \
      test/with_dummyserver/test_proxy_poolmanager.py::TestIPv6HTTPProxyManager::test_basic_ipv6_proxy
    --deselect \
      test/with_dummyserver/test_socketlevel.py::TestSSL::test_ssl_failure_midway_through_conn
    --deselect \
      test/with_dummyserver/test_socketlevel.py::TestProxyManager::test_https_proxymanager_connected_to_http_proxy
    --deselect \
      test/with_dummyserver/test_socketlevel.py::TestSSL::test_retry_ssl_error
    --deselect \
      test/with_dummyserver/test_socketlevel.py::TestSSL::test_ssl_failed_fingerprint_verification
    --deselect \
      test/with_dummyserver/test_socketlevel.py::TestSSL::test_ssl_custom_validation_failure_terminates
    --deselect \
      test/with_dummyserver/test_socketlevel.py::TestSSL::test_requesting_large_resources_via_ssl
    --deselect \
      test/test_connection.py::TestConnection::test_recent_date
    --deselect \
      test/test_no_ssl.py::TestImportWithoutSSL::test_cannot_import_ssl
    --deselect \
      test/contrib/test_pyopenssl.py::TestSSL::test_ssl_failed_fingerprint_verification
    --deselect \
      test/contrib/test_pyopenssl.py::TestSSL::test_ssl_custom_validation_failure_terminates
  )
  cd \
    "${_pkg}-${pkgver}"
  # install to temporary location, as importlib is used
  "${_py}" \
    -m \
      installer \
    --destdir=test_dir \
    dist/*.whl
  export \
    PYTHONPATH="${PWD}/test_dir/${_site_packages}:${PYTHONPATH}"
  pytest \
    "${pytest_options[@]}"
}

package() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  install \
    -Dm644 \
    LICENSE.txt \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
}
