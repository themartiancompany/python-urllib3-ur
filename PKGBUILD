# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Patrice Peterson <runiq at archlinux dot us>
# Contributor: Chris Brannon <cmbrannon79@gmail.com>
# Contributor: BorgHunter <borghunter at gmail dot com>

pkgbase=python-urllib3
pkgname=(python-urllib3 python2-urllib3)
pkgver=1.12
_commit=e91c16169e463118ce662345461933bb3a7dedef
pkgrel=2
pkgdesc="HTTP library with thread-safe connection pooling and file post support"
arch=("any")
url="https://github.com/shazow/urllib3"
license=("MIT")
makedepends=('python-setuptools' 'python2-setuptools' 'git')
checkdepends=('python-nose' 'python2-nose' 'python-mock' 'python2-mock' 'python-coverage' 'python2-coverage'
              'python-tornado' 'python2-tornado' 'twine' 'python2-twine' 'python2-pyopenssl')
source=("git+https://github.com/shazow/urllib3.git#commit=$_commit")
md5sums=('SKIP')

prepare() {
  cp -a urllib3{,-py2}
}

build() {
  cd urllib3
  python setup.py build

  cd ../urllib3-py2
  python2 setup.py build
}

check() {
  # Expected failure when asking for external resources

  cd urllib3
  nosetests3 || warning "Tests failed"

  cd ../urllib3-py2
  nosetests2 || warning "Tests failed"
}

package_python-urllib3() {
  depends=('python')

  cd urllib3
  python setup.py install --root="${pkgdir}"
  install -Dm644 LICENSE.txt "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.txt"
}

package_python2-urllib3() {
  depends=('python2')

  cd urllib3-py2
  python2 setup.py install --root="${pkgdir}"
  install -Dm644 LICENSE.txt "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.txt"
}
