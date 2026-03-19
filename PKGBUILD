#https://github.com/calamares/calamares/releases
#change prepare number too

pkgname=calamaresx-app
_pkgname=calamares
pkgver=3.4.3
pkgrel=2
pkgdesc='Distribution-independent installer framework'
arch=('x86_64')
license=(GPL)
url="https://github.com/XeroLinuxDev/calamaresx-app.git"
license=('LGPL')
provides=('calamares')
depends=(
  boost-libs
  ckbcomp
  cryptsetup
  dmidecode
  gptfdisk
  hwinfo
  kconfig
  kcoreaddons
  kcrash
  ki18n
  kparts
  kpmcore
  kservice
  kwidgetsaddons
  libpwquality
  mkinitcpio-openswap
  networkmanager
  polkit-qt6
  python
  qt6-declarative
  qt6-svg
  rsync
  solid
  squashfs-tools
  upower
  yaml-cpp
)
makedepends=(
  boost
  cmake
  doxygen
  extra-cmake-modules
  git
  ninja
  python-jsonschema
  python-pyaml
  python-unidecode
  qt6-tools
)
# backup=('usr/share/calamares/modules/bootloader.conf'
#         'usr/share/calamares/modules/displaymanager.conf'
#         'usr/share/calamares/modules/initcpio.conf'
#         'usr/share/calamares/modules/unpackfs.conf')

# Files calamares.desktop and calamares_polkit are included in the git repo
source=("$pkgname::git+$url")

sha256sums=('SKIP')

prepare() {

# 	cp -rv ../modules/* ${srcdir}/$pkgname/src/modules/
#
# 	sed -i 's/command\.append("-S")/command.append("-Syy")/' "$srcdir/$pkgname/src/modules/packages/main.py"
# 	sed -i -e 's/"Install configuration files" OFF/"Install configuration files" ON/' "$srcdir/$pkgname/CMakeLists.txt"
# # 	sed -i '/case FileSystem::Xfs:/ a case FileSystem::Bcachefs:' "$srcdir/$pkgname/src/libcalamares/partition/FileSystem.cpp"
# #   sed -i '/case FileSystem::Fat16:/ i case FileSystem::Bcachefs:' "$srcdir/$pkgname/src/modules/partition/core/PartitionLayout.cpp"
# 	sed -i -e "s/desired_size = 512 \* 1024 \* 1024  \# 512MiB/desired_size = 512 \* 1024 \* 1024 \* 4  \# 2048MiB/" "$srcdir/$pkgname/src/modules/fstab/main.py"

	cd $pkgname
	sed -i -e "s|CALAMARES_VERSION 3.4.1|CALAMARES_VERSION $pkgver-$pkgrel|g" CMakeLists.txt
}

build() {
	cd $pkgname

    cmake -S . -Bbuild \
        -GNinja \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DCMAKE_INSTALL_LIBDIR=lib \
        -DWITH_APPSTREAM=OFF \
        -DWITH_PYBIND11=OFF \
        -DWITH_QT6=ON \
        -DSKIP_MODULES="dracut \
            dracutlukscfg \
            dummycpp \
            dummyprocess \
            dummypython \
            dummypythonqt \
            initramfs \
            initramfscfg \
            interactiveterminal \
            keyboardq \
            license \
            localeq \
            oemid \
            packagechooserq \
            partitionq \
            services-openrc \
            summaryq \
            tracking \
            welcomeq \
            zfs \
            zfshostid"

    cmake --build build
}

package() {
	cd $pkgname/build
	DESTDIR="${pkgdir}" cmake --build . --target install

	# Install desktop file and polkit helper from repo root
	install -Dm755 "$srcdir/$pkgname/calamares.desktop" "$pkgdir/home/liveuser/Desktop/calamares.desktop"
	install -Dm755 "$srcdir/$pkgname/calamares_polkit" "$pkgdir/usr/bin/calamares_polkit"

	# Remove default desktop file (we use our custom one)
	rm -f "$pkgdir/usr/share/applications/calamares.desktop"
}
