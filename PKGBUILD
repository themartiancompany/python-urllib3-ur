# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Patrice Peterson <runiq at archlinux dot us>
# Contributor: Chris Brannon <cmbrannon79@gmail.com>
# Contributor: BorgHunter <borghunter at gmail dot com>

_name=urllib3
pkgname=python-urllib3
pkgver=1.26.18
pkgrel=2
pkgdesc="HTTP library with thread-safe connection pooling and file post support"
arch=("any")
url="https://github.com/urllib3/urllib3"
license=("MIT")
depends=('python')
makedepends=(
  'python-build'
  'python-installer'
  'python-setuptools'
  'python-wheel'
)
checkdepends=(
  'python-brotli'
  'python-certifi'
  'python-cryptography'
  'python-dateutil'
  'python-flaky'
  'python-gcp-devrel-py-tools'
  'python-idna'
  'python-pyopenssl'
  'python-pysocks'
  'python-pytest'
  'python-pytest-freezegun'
  'python-pytest-timeout'
  'python-tornado'
  'python-trustme'
)
optdepends=(
  'python-brotli: Brotli support'
  'python-certifi: security support'
  'python-cryptography: security support'
  'python-idna: security support'
  'python-pyopenssl: security support'
  'python-pysocks: SOCKS support'
)
source=("https://github.com/urllib3/urllib3/archive/$pkgver/$pkgname-$pkgver.tar.gz")
sha512sums=('62c0af4b11e797a85420ef3f0888f2e608334329eddd88b9fe563b5437189cbea8dbbcd53f999557d9828fcf4bf03b8ca9f6e3d401533bc4ae8ff96e3ece1557')

prepare() {
  # remove python-mock requirement
  find $_name-$pkgver -type f -iname "*.py" -exec sed 's/import mock/from unittest import mock/; s/from mock/from unittest.mock/' -i {} +
}

build() {
  cd $_name-$pkgver
  python -m build --wheel --no-isolation
}

check() {
  local pytest_options=(
    -vv
    # TODO: report upstream
    --deselect test/test_ssltransport.py::SingleTLSLayerTestCase::test_ssl_object_attributes
    --deselect test/contrib/test_pyopenssl.py::TestSSL::test_ssl_read_timeout
    --deselect test/with_dummyserver/test_socketlevel.py::TestSSL::test_ssl_read_timeout
  )
  local site_packages=$(python -c "import site; print(site.getsitepackages()[0])")

  cd $_name-$pkgver
  # install to temporary location, as importlib is used
  python -m installer --destdir=test_dir dist/*.whl
  export PYTHONPATH="$PWD/test_dir/$site_packages:$PYTHONPATH"
  pytest "${pytest_options[@]}"
}

package() {
  cd $_name-$pkgver
  python -m installer --destdir="$pkgdir" dist/*.whl
  install -Dm644 LICENSE.txt -t "$pkgdir"/usr/share/licenses/$pkgname/
}
