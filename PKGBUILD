# $Id: PKGBUILD 158661 2012-05-05 22:14:03Z eric $
# Maintainer: Eric Bélanger <eric@archlinux.org>

pkgname=sysklogd
pkgver=1.5
pkgrel=4
pkgdesc="System and kernel log daemons"
arch=('i686' 'x86_64')
url="http://www.infodrom.org/projects/sysklogd/"
license=('GPL' 'BSD')
depends=('glibc' 'bash')
provides=('logger')
backup=('etc/syslog.conf' 'etc/logrotate.d/syslog')
source=(http://www.infodrom.org/projects/sysklogd/download/${pkgname}-${pkgver}.tar.gz{,.asc} \
        syslog.conf syslog.logrotate syslogd klogd LICENSE \
        sysklogd-1.4.1-caen-owl-syslogd-bind.diff \
        sysklogd-1.4.1-caen-owl-syslogd-drop-root.diff \
        sysklogd-1.4.1-caen-owl-klogd-drop-root.diff \
        sysklogd-1.5-syslog-func-collision.patch)
sha1sums=('070cce745b023f2ce7ca7d9888af434d6d61c236'
          '9599322fc176004d95b5111b05f665b5191dfe67'
          '35b4cb76109a6ffe9269021a6bfb4f8da614a4eb'
          'e67c0f78f13c94507d3f686b4e5b8340db4624fd'
          '848beb23b9ca4de19c6022df03878dbe57e04c0a'
          'f46088f761c033562a59bc13d4888b7343bc02fc'
          'c416bcefd3d3d618139cc7912310caddf34c0c0b'
          '849b2dcaf11060d583ccb3c48356a6971df45cf0'
          '9701989490748b0c5a1727e0fc459179d0e350a8'
          '76da0ecd9bca969e292a6ec58d7cd96e4c97e525'
          '826e76a59834868658eb9f8d8f3aabd8bf748759')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
# CAEN/OWL security patches
  patch -p1 -i ../sysklogd-1.4.1-caen-owl-syslogd-bind.diff
  patch -p1 -i ../sysklogd-1.4.1-caen-owl-syslogd-drop-root.diff 
  patch -p1 -i ../sysklogd-1.4.1-caen-owl-klogd-drop-root.diff

  patch -p1 -i ../sysklogd-1.5-syslog-func-collision.patch
  sed -i -e "s/-O3/${CFLAGS} -D_FILE_OFFSET_BITS=64 -D_LARGEFILE_SOURCE/" \
         -e "s/LDFLAGS= -s/LDFLAGS= ${LDFLAGS}/" Makefile
  sed -i 's/500 -s/755/' Makefile
  make
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  install -d "${pkgdir}/usr/sbin" "${pkgdir}"/usr/share/man/{man5,man8}
  make prefix="${pkgdir}" install
  install -D -m644 ../syslog.conf "${pkgdir}/etc/syslog.conf"
  install -D -m644 ../syslog.logrotate "${pkgdir}/etc/logrotate.d/syslog"
  install -D -m755 ../syslogd "${pkgdir}/etc/rc.d/syslogd"
  install -D -m755 ../klogd "${pkgdir}/etc/rc.d/klogd"
  install -D -m644 ../LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
