Adapted from Nelson Tull's instructions for installing GRASS GIS on Ubuntu 20.04

Follow these install commands for GRASS on Ubuntu (don't need optional commands, see below for required steps): https://grasswiki.osgeo.org/wiki/Compile_and_Install_Ubuntu

```sh
$ sudo apt update && sudo apt upgrade -y
$ sudo apt install libproj-dev proj-data proj-bin unzip libgeos-dev \
 build-essential \
 flex make bison gcc libgcc1 g++ ccache \
 python3 python3-dev \
 python3-opengl python3-wxgtk4.0 \
 python3-dateutil libgsl-dev python3-numpy \
 wx3.0-headers wx-common libwxgtk3.0-gtk3-dev \
 libwxbase3.0-dev   \
 libncurses5-dev \
 libbz2-dev \
 zlib1g-dev gettext \
 libtiff5-dev libpnglite-dev \
 libcairo2 libcairo2-dev \
 sqlite3 libsqlite3-dev \
 libpq-dev \
 libreadline6-dev libfreetype6-dev \
 libfftw3-3 libfftw3-dev \
 libboost-thread-dev libboost-program-options-dev  libpdal-dev\
 subversion libzstd-dev \
 checkinstall \
 libglu1-mesa-dev libxmu-dev \
 ghostscript wget -y
```

This next step might no longer be necessary. First try skipping to the next step (Install GRASS and dependencies)
```sh
$ cd $HOME/Software # (or wherever)
$ git clone https://github.com/OSGeo/grass.git grass-7.8.latest.git
$ cd grass-7.8.latest.git/
$ git checkout releasebranch_7_8
$ MYCFLAGS='-O2 -fPIC -fno-common -fexceptions -std=gnu99 -fstack-protector -m64'
$ MYLDFLAGS='-Wl,--no-undefined -Wl,-z,now'
$ LDFLAGS="$MYLDFLAGS" CFLAGS="$MYCFLAGS" CXXFLAGS="$MYCXXFLAGS" ./configure \
  --with-cxx \
  --enable-largefile \
  --with-proj --with-proj-share=/usr/share/proj \
  --with-gdal=/usr/bin/gdal-config \
  --with-python \
  --with-geos \
  --with-sqlite \
  --with-nls \
  --with-zstd \
  --with-pdal \
  --with-cairo --with-cairo-ldflags=-lfontconfig \
  --with-freetype=yes --with-freetype-includes="/usr/include/freetype2/" \
  --with-wxwidgets \
  --with-fftw \
  --with-motif \
  --with-opengl-libs=/usr/include/GL \
  --with-postgres=yes --with-postgres-includes="/usr/include/postgresql" \
  --without-netcdf \
  --without-mysql \
  --without-odbc \
  --without-openmp \
  --without-ffmpeg
$ make -j4 # (will take a while)
$ sudo make install # (wait to launch grass78 until some other issues are addressed)
```

Install GRASS and dependencies:

```sh
$ sudo apt install --fix-missing grass grass-doc grass-dev \
 subversion libcanberra-gtk-module python3-distutils -y
```

Optionally create user location and mapset data (use your grass version, grass78 or grass79):

```sh
$ cd $HOME/Documents
$ mkdir grassdata
$ grass78 -e -c /path/to/DEM/[MyDEM].tif $HOME/Documents/grassdata/[MyLocationName]
$ grass78 -c $HOME/Documents/grassdata/[MyLocationName]/[MyMapsetName]
```
