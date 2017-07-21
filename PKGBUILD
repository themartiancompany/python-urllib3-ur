# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Patrice Peterson <runiq at archlinux dot us>
# Contributor: Chris Brannon <cmbrannon79@gmail.com>
# Contributor: BorgHunter <borghunter at gmail dot com>

pkgbase=python-urllib3
pkgname=(python-urllib3 python2-urllib3 python-urllib3-doc)
pkgver=1.22
pkgrel=1
pkgdesc="HTTP library with thread-safe connection pooling and file post support"
arch=("any")
url="https://github.com/shazow/urllib3"
license=("MIT")
makedepends=('python-setuptools' 'python2-setuptools' 'python2-sphinx' 'python-ndg-httpsclient'
             'python2-ndg-httpsclient' 'python-pyasn1' 'python2-pyasn1' 'python-pyopenssl'
             'python2-pyopenssl' 'python-pysocks' 'python2-pysocks' 'python-mock' 'python2-mock')
checkdepends=('python-pytest-runner' 'python2-pytest-runner' 'python-tornado' 'python2-tornado'
              'python-nose' 'python2-nose' 'python-psutil' 'python2-psutil'
              'python-gcp-devrel-py-tools' 'python2-gcp-devrel-py-tools')
source=("$pkgbase-$pkgver.tar.gz::https://github.com/shazow/urllib3/archive/$pkgver.tar.gz"
        tornado-4.3.patch)
sha512sums=('1b45a4a64e71847a4fc62b9263235d5b05b62076698fa324454efeb7ad065abd702cc9eadb2d396d9270b07e91e9bad94c52a4b9b115aadccb27f81955e6feab'
            '7c09acefa963a80379f8b2f3f2c2c7546ec62025058c1ae024bc954d49392d7956b8b3ceaed40b3d3ab06bcf9c74bfb4214425b66cc55c50ffc2642e2d35c498')

prepare() {
  # https://github.com/shazow/urllib3/pull/1236
  (cd urllib3-$pkgver; patch -p1 -i ../tornado-4.3.patch)

  cp -a urllib3-$pkgver{,-py2}
}

build() {
  cd "$srcdir"/urllib3-$pkgver
  python setup.py build

  cd "$srcdir"/urllib3-$pkgver-py2
  python2 setup.py build

  # Build with Python 2 since autodoc produces errors on Python 3
  cd "$srcdir"/urllib3-$pkgver/docs
  make SPHINXBUILD=sphinx-build2 html
}

check() {
  cd "$srcdir"/urllib3-$pkgver
  python setup.py pytest

  cd "$srcdir"/urllib3-$pkgver-py2
  python2 setup.py pytest
}

package_python-urllib3() {
  depends=('python')
  optdepends=('python-pysocks: SOCKS support')

  cd urllib3-$pkgver
  python setup.py install --root="$pkgdir"
  install -Dm644 LICENSE.txt "$pkgdir"/usr/share/licenses/$pkgname/LICENSE.txt
}

package_python2-urllib3() {
  depends=('python2')
  optdepends=('python2-pysocks: SOCKS support')

  cd urllib3-$pkgver-py2
  python2 setup.py install --root="$pkgdir"
  install -Dm644 LICENSE.txt "$pkgdir"/usr/share/licenses/$pkgname/LICENSE.txt
}

package_python-urllib3-doc() {
  pkgdesc="urllib3 Documentation"

  cd urllib3-$pkgver/docs
  install -d "$pkgdir"/usr/share/doc
  cp -r _build/html "$pkgdir"/usr/share/doc/python-urllib3
  install -Dm644 ../LICENSE.txt "$pkgdir"/usr/share/licenses/$pkgname/LICENSE.txt
}
