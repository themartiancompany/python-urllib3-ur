# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Patrice Peterson <runiq at archlinux dot us>
# Contributor: Chris Brannon <cmbrannon79@gmail.com>
# Contributor: BorgHunter <borghunter at gmail dot com>

pkgbase=python-urllib3
pkgname=(python-urllib3 python2-urllib3 python-urllib3-doc)
pkgver=1.18
pkgrel=1
pkgdesc="HTTP library with thread-safe connection pooling and file post support"
arch=("any")
url="https://github.com/shazow/urllib3"
license=("MIT")
makedepends=('python-setuptools' 'python2-setuptools' 'python2-sphinx' 'python-ndg-httpsclient'
             'python2-ndg-httpsclient' 'python-pyasn1' 'python2-pyasn1' 'python-pyopenssl'
             'python2-pyopenssl' 'python-pysocks' 'python2-pysocks' 'git')
checkdepends=('python-nose' 'python2-nose' 'python-mock' 'python2-mock'
              'python-tornado' 'python2-tornado' 'python-coverage' 'python2-coverage')
source=("git+https://github.com/shazow/urllib3.git#tag=$pkgver")
md5sums=('SKIP')

prepare() {
  cp -a urllib3{,-py2}
}

build() {
  cd "$srcdir"/urllib3
  python setup.py build

  cd "$srcdir"/urllib3-py2
  python2 setup.py build

  # Build with Python 2 since autodoc produces errors on Python 3
  cd "$srcdir"/urllib3/docs
  make SPHINXBUILD=sphinx-build2 html
}

check() {
  # Failures are related to missing SSLv3

  cd "$srcdir"/urllib3
  nosetests3 || warning "Tests failed"

  cd "$srcdir"/urllib3-py2
  nosetests2 || warning "Tests failed"
}

package_python-urllib3() {
  depends=('python')
  optdepends=('python-pysocks: SOCKS support')

  cd urllib3
  python setup.py install --root="$pkgdir"
  install -Dm644 LICENSE.txt "$pkgdir"/usr/share/licenses/$pkgname/LICENSE.txt
}

package_python2-urllib3() {
  depends=('python2')
  optdepends=('python2-pysocks: SOCKS support')

  cd urllib3-py2
  python2 setup.py install --root="$pkgdir"
  install -Dm644 LICENSE.txt "$pkgdir"/usr/share/licenses/$pkgname/LICENSE.txt
}

package_python-urllib3-doc() {
  pkgdesc="urllib3 Documentation"

  cd urllib3/docs
  install -d "$pkgdir"/usr/share/doc
  cp -r _build/html "$pkgdir"/usr/share/doc/python-urllib3
  install -Dm644 ../LICENSE.txt "$pkgdir"/usr/share/licenses/$pkgname/LICENSE.txt
}
