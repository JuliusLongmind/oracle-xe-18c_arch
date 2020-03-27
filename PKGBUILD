# Maintainer: Maxim Kurnosenko <asusx2@mail.ru>
# Contributor: RonaldMcDaddy <wannes.demeyer@protonmail.com>
# Contributor: Tinh Truong <xuantinh at gmail dot com>
# Contributor: Cedric Sougne <cedric@sougne.name>
# Contributor: untseac
# Contributor: siasia
# Contributor: Lukas Jirkovsky <l.jirkovsky@gmail.com>

pkgname=oracle-xe
pkgver=18.4.0_1.0
pkgrel=1
pkgdesc="a non free DBMS"
url="http://www.oracle.com/"
license=('custom')
arch=('x86_64')
conflicts=('oracle-xe')
provides=('oracle-xe')
options=('!strip')
depends=('libaio>=0.3.112-2' 'gcc>=9.3.0-1' 'binutils>=2.34-2.1' 'make>=4.3-1' 'glibc>=2.31-2' 'bc' 'net-tools')
install='oracle-xe.install'
source=(
        'manual://download/file/from/oracle/page/oracle-database-xe-18c-1.0-1.x86_64.rpm'
        'oracle_env.csh'
        'oracle_env.sh'
        'oracle-xe-18c'
        'oracle-xe-18c.conf'
        'oracle-xe-18c.ld.so.conf'
        'oracle-xe.service'
)

DLAGENTS+=('manual::/usr/bin/echo The source file for this package needs to be downloaded manually, since it requires a login and is not redistributable. Please visit http://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/index.html')

md5sums=(
        '9ff73be7e89edb22e5c1f7a457978aca'
        '648ee79a2a72beb83195288df0e5a544'
        'e3947cdec45dbf2d357f6ff25559ac15'
        '742a745373d1f873c3417e1defafd92e'
        '9ce538757edbdb49847677ef6520de5b'
        '9f0c964a1b046e8d930549915b468e35'
        '3dd923ac2df9fd38827fc9fc0048273a'
)

build() {
    cd $srcdir
#    bsdtar -xf oracle-xe-18.4.0-1.0.x86_64.rpm
}

package() {
    cd $srcdir

    mkdir -p $pkgdir/etc/rc.d
    cp $srcdir/oracle-xe-18c $pkgdir/etc/rc.d/
    chmod +x $pkgdir/etc/rc.d/oracle-xe-18c

    #Fix for ***[FATAL] [DBT-50000] Unable to check for available memory.****
    corr1="s_-sampleSchema_-J-Doracle.assistants.dbca.validate.ConfigurationParams=false -sampleSchema_g"
    sed -i "${corr1}" $pkgdir/etc/rc.d/oracle-xe-18c

    mkdir -p $pkgdir/etc/sysconfig
    cp $srcdir/oracle-xe-18c.conf $pkgdir/etc/sysconfig

    mkdir -p $pkgdir/opt
    mv $srcdir/opt/oracle $pkgdir/opt

    find $pkgdir -exec chmod 755 {} \;

    # Export environment variables
    mkdir -p $pkgdir/etc/profile.d
    cp $srcdir/oracle_env.* $pkgdir/etc/profile.d/
    chmod +x $pkgdir/etc/profile.d/oracle_env.*

    # Desktop files
    cp -a $srcdir/usr $pkgdir

    # LD_LIBRARY_PATH
    mkdir -p $pkgdir/etc/ld.so.conf.d/
    cp $srcdir/oracle-xe-18c.ld.so.conf $pkgdir/etc/ld.so.conf.d/oracle-xe-18c.conf

    # License
    mkdir -p $pkgdir/usr/share/licenses/custom/$pkgname
    cp $srcdir/usr/share/doc/oracle-xe-18c/LICENSE $pkgdir/usr/share/licenses/custom/$pkgname

    # For systemd
    mkdir -p $pkgdir/etc/systemd/system
    cp $srcdir/oracle-xe.service $pkgdir/etc/systemd/system
}
