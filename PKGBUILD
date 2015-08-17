# Maintainer: chris_L <chrislcenter dash mamoru at yahoo.com dot mx>

pkgname=wolf4sdl-svn
pkgver=255
pkgrel=3
pkgdesc='An enhanced SDL port of the FPS "Wolfenstein 3D" by id Software (You need the retail game files to play), svn version'
arch=('i686' 'x86_64')
url="http://www.chaos-software.de.vu/"
license=('custom')
depends=('sdl_mixer' 'gcc-libs')
makedepends=('subversion' 'pkgconfig' 'intltool' 'sed')
conflicts=('wolf4sdl')
provides=('wolf4sdl')
install=$pkgname.install
source=(http://www.fileden.com/files/2010/12/31/3049394//pc-speaker-by-ljbade-build255.patch)
md5sums=('6dc6ec5817e2c6a483a2d3585ed7d002')


_svntrunk=svn://tron.homeunix.org:3690/wolf3d/trunk
_svnmod=wolf4sdl

build() {
	cd $startdir/src
	svn co $_svntrunk -r $pkgver $_svnmod
	cd $_svnmod
	msg "Starting build..."
	patch -p1 < ../pc-speaker-by-ljbade-build255.patch # patch for pc-speaker emulation, by ljbade

	# By default, it expects the GT/ID/Activision 1.4 full version files, 
	# uncomment one of the following lines if you got errors about numchecks and you are not using a mod
	# sed -i -e 's/#define GOODTIMES/\/\/&/' -e 's/\/\/\(#define APOGEE_1_1\)/\1/' version.h # uncoment this line if you have the v1.1 apogee full version
	# sed -i -e 's/#define GOODTIMES/\/\/&/' version.h # uncoment this line if you have the v1.4 apogee full version


	# sed lines to make it use /usr/share/wolf3d for data files
	find -type f |xargs -I file sed -i 's/"\([a-z0-9][a-z0-9]*\.wl[1-9]\)/"\/usr\/share\/wolf3d\/\1/g' file
	sed -i 's/\(\[\].*\)"\([a-z0-9][a-z0-9]*\.\)"/\1"\/usr\/share\/wolf3d\/\2"/g' id_ca.cpp
	sed -i 's/\(strcpy(fname, "\)\(gamemaps."\)/\1\/usr\/share\/wolf3d\/\2/' id_ca.cpp
	sed -i 's/^.*fname\[13\] = "vswap\..*/static const char gvswap\[\] = "\/usr\/share\/wolf3d\/vswap."; char fname\[\]=""; strcpy(fname,gvswap);/' id_pm.cpp
	sed -i 's/if(!stat("vswap.wl\([0-9]\)", &statbuf))/if(!stat("\/usr\/share\/wolf3d\/vswap.wl\1", \&statbuf))/' wl_menu.cpp

	make BINARY=wolf4sdl || return 1
	make PREFIX="$pkgdir"/usr BINARY=wolf4sdl install

	mkdir -p "$pkgdir"/usr/share/{wolf3d,licenses/wolf4sdl}
	install -m644 license*.txt "$pkgdir"/usr/share/licenses/wolf4sdl/
}


