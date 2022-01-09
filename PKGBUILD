# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Patrice Peterson <runiq at archlinux dot us>
# Contributor: Chris Brannon <cmbrannon79@gmail.com>
# Contributor: BorgHunter <borghunter at gmail dot com>

pkgbase=python-urllib3
pkgname=(python-urllib3 python-urllib3-doc)
pkgver=1.26.8
pkgrel=1
pkgdesc="HTTP library with thread-safe connection pooling and file post support"
arch=("any")
url="https://github.com/urllib3/urllib3"
license=("MIT")
makedepends=('python-setuptools' 'python-sphinx' 'python-ndg-httpsclient'
             'python-pyasn1' 'python-pyopenssl'
             'python-pysocks' 'python-mock'
             'python-brotli' 'python-sphinx-furo')
checkdepends=('python-pytest-runner' 'python-tornado' 'python-nose' 'python-psutil' 'python-trustme'
              'python-gcp-devrel-py-tools' 'python-flaky' 'python-dateutil')
source=("https://github.com/urllib3/urllib3/archive/$pkgver/$pkgbase-$pkgver.tar.gz"
        urllib3-use-brotli-or-brotli-cffi.patch::https://github.com/urllib3/urllib3/pull/2099.patch)
sha512sums=('7ba3c3f48315d0f2ef3f2fbdef2b9e508a4af4e9c9f805b96bab2f47d5d35dc29672a16f2163a0ba375ea01bbc005052481ead37dfdcba18f22e90e03616b6d5'
            '08b58960410a996b039eb3f46da252703055d79228733c65fbbe8d31fedd5b3956670230602deabf02407cac5f5d425b8d65bf3b16bdecd38f2541c6c9c82934')

prepare() {
  patch -d urllib3-$pkgver -p1 -i ../urllib3-use-brotli-or-brotli-cffi.patch || :
  cp -a urllib3-$pkgver{,-py2}
}

build() {
  cd "$srcdir"/urllib3-$pkgver
  python setup.py build

  cd "$srcdir"/urllib3-$pkgver/docs
  make html
}

check() {
  cd urllib3-$pkgver
  # TODO: investigate test_respect_retry_after_header_sleep
  python setup.py pytest --addopts "--deselect test/test_retry.py::TestRetry::test_respect_retry_after_header_sleep --deselect test/test_retry_deprecated.py::TestRetry::test_respect_retry_after_header_sleep"
}

package_python-urllib3() {
  depends=('python')
  optdepends=('python-pysocks: SOCKS support'
              'python-brotli: Brotli support'
              'python-pyopenssl: security support'
              'python-idna: security support')

  cd urllib3-$pkgver
  python setup.py install --root="$pkgdir"
  install -Dm644 LICENSE.txt "$pkgdir"/usr/share/licenses/$pkgname/LICENSE.txt
}

package_python-urllib3-doc() {
  pkgdesc="urllib3 Documentation"

  cd urllib3-$pkgver/docs
  install -d "$pkgdir"/usr/share/doc
  cp -r _build/html "$pkgdir"/usr/share/doc/python-urllib3
  install -Dm644 ../LICENSE.txt "$pkgdir"/usr/share/licenses/$pkgname/LICENSE.txt
}
