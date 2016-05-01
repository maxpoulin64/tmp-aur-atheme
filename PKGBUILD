# Maintainer: Maxime Poulin <maxpoulin64@gmail.com>

# Build options (yes/no/default)
_large_network=default
_pcre=default
_perl=default
_contrib=default
_cracklib=default
_ldap=default

pkgname=atheme
pkgver=7.2.6
pkgrel=1
pkgdesc="Atheme IRC services"
arch=("i686" "x86_64")
url="http://www.atheme.org/atheme/"
license=('MIT')
depends=('bash')
conflicts=('libmowgli' 'atheme')
optdepends=(
	'pcre: for pcre support'
	'perl: for Perl support'
	'cracklib: cracklib support in NickServ'
	'libldap: LDAP support'
)
backup=(
	'etc/atheme/atheme.conf'
	'etc/atheme/atheme.motd'
)
install=atheme.install
source=(
	"atheme::git+https://github.com/atheme/atheme.git#tag=atheme-$pkgver"
	"system.service"
)
md5sums=("SKIP" "SKIP")

prepare() {
	cd "$srcdir/atheme"
	git submodule update --init
}

build() {
	cd "$srcdir/atheme"
	
	_configure="./configure --prefix=/usr --enable-fhs-paths"
	_configure+=" --sysconfdir=/etc/atheme --localstatedir=/var"
	
	[[ $_large_network = yes ]] && _configure+=" --enable-large-net"
	[[ $_contrib = yes ]]       && _configure+=" --enable-contrib"
	
	[[ $_pcre = yes ]] && _configure+=" --with-pcre"
	[[ $_pcre = no ]]  && _configure+=" --without-pcre"
	

	[[ $_pcre = yes ]] && _configure+=" --with-pcre"
	[[ $_pcre = no ]]  && _configure+=" --without-pcre"

	eval $_configure
	make
}

package() {
	cd "$srcdir/atheme"
	make DESTDIR="$pkgdir/" install
	install -Dm0644 "$srcdir/system.service" "$pkgdir/usr/lib/systemd/system/atheme.service"
	
	cd "$pkgdir"
	rm -r var
	install -dm755 -o142 -g142 var/lib/atheme/
	
	chmod 0755 etc/atheme/
	chmod 0644 etc/atheme/*
	install -dm755 usr/share/doc/atheme/config/
	mv etc/atheme/atheme.conf.example etc/atheme/atheme.conf
	mv etc/atheme/atheme.conf.operserv-example usr/share/doc/atheme/config/
	mv etc/atheme/atheme.conf.userserv-example usr/share/doc/atheme/config/
	mv etc/atheme/atheme.motd.example etc/atheme/atheme.motd
	rm etc/atheme/atheme.cron.example
}
