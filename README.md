GEOS -- Geometry Engine, Open Source
====================================

Project homepage: http://geos.osgeo.org/

## Build status

| branch / CI | Debbie | Winnie | Dronie | Travis CI | GitLab CI | AppVeyor | Bessie | Bessie32 |
|:---         |:---    |:---    |:---    |:---       |:---       |:---      |:---    |:---    |
| master      | [![debbie](https://debbie.postgis.net/buildStatus/icon?job=GEOS_Master)](https://debbie.postgis.net/view/GEOS/job/GEOS_Master/) | [![winnie](https://winnie.postgis.net:444/view/GEOS/job/GEOS_Master/badge/icon)](https://winnie.postgis.net:444/view/GEOS/job/GEOS_Master/) | [![dronie](https://drone.osgeo.org/api/badges/geos/geos/status.svg?branch=master)](https://drone.osgeo.org/geos/geos?branch=master) | [![travis](https://travis-ci.com/libgeos/geos.svg?branch=master)](https://travis-ci.com/libgeos/geos?branch=master) | [![gitlab-ci](https://gitlab.com/geos/libgeos/badges/master/build.svg)](https://gitlab.com/geos/libgeos/commits/master) | [![appveyor](https://ci.appveyor.com/api/projects/status/62aplwst722b89au/branch/master?svg=true)](https://ci.appveyor.com/project/dbaston/geos/branch/master) | [![bessie](https://debbie.postgis.net/buildStatus/icon?job=GEOS_Worker_Run/label=bessie&BRANCH=master)](https://debbie.postgis.net/view/GEOS/job/GEOS_Worker_Run/label=bessie) | [![bessie32](https://debbie.postgis.net/buildStatus/icon?job=GEOS_Worker_Run/label=bessie32&BRANCH=master)](https://debbie.postgis.net/view/GEOS/job/GEOS_Worker_Run/label=bessie32) ||
| 3.7     | [![debbie](https://debbie.postgis.net/buildStatus/icon?job=GEOS_Branch_3.7)](https://debbie.postgis.net/view/GEOS/job/GEOS_Branch_3.7/) | [![winnie](https://winnie.postgis.net:444/view/GEOS/job/GEOS_Branch_3.7/badge/icon)](https://winnie.postgis.net:444/view/GEOS/job/GEOS_Branch_3.7/) | [![dronie](https://drone.osgeo.org/api/badges/geos/geos/status.svg?branch=3.7)](https://drone.osgeo.org/geos/geos?branch=3.7) | [![travis](https://travis-ci.com/libgeos/geos.svg?branch=3.7)](https://travis-ci.com/libgeos/geos?branch=3.7) | [![gitlab-ci](https://gitlab.com/geos/libgeos/badges/svn-3.7/build.svg)](https://gitlab.com/geos/libgeos/commits/3.7) | [![appveyor](https://ci.appveyor.com/api/projects/status/62aplwst722b89au/branch/3.7?svg=true)](https://ci.appveyor.com/project/dbaston/geos/branch/3.7) ||
| 3.6     | [![debbie](https://debbie.postgis.net/buildStatus/icon?job=GEOS_Branch_3.6)](https://debbie.postgis.net/view/GEOS/job/GEOS_Branch_3.6/) | [![winnie](https://winnie.postgis.net:444/view/GEOS/job/GEOS_Branch_3.6/badge/icon)](https://winnie.postgis.net:444/view/GEOS/job/GEOS_Branch_3.6/) | [![dronie](https://drone.osgeo.org/api/badges/geos/geos/status.svg?branch=svn-3.6)](https://drone.osgeo.org/geos/geos?branch=svn-3.6) | [![travis](https://travis-ci.com/libgeos/geos.svg?branch=svn-3.6)](https://travis-ci.com/libgeos/geos?branch=svn-3.6) | [![gitlab-ci](https://gitlab.com/geos/libgeos/badges/svn-3.6/build.svg)](https://gitlab.com/geos/libgeos/commits/svn-3.6) | [![appveyor](https://ci.appveyor.com/api/projects/status/62aplwst722b89au/branch/svn-3.6?svg=true)](https://ci.appveyor.com/project/dbaston/geos/branch/svn-3.6) |

More on: https://trac.osgeo.org/geos#BuildandInstall

## Building, testing, installing

### Prerequisites

Building GEOS requires a C++11 compiler

### Unix

#### Gen Buildchain

##### Using Autotools:

    ./autogen.sh  # in ${srcdir}, if obtained from SVN or GIT
    (mkdir obj && cd obj && ../configure)

##### Using CMake:

    (mkdir build && cd build && cmake ..)

#### Build (both Autotools and CMake)

    make
    make check
    sudo make install # (as root, assuming PREFIX is not writable by the build user)
    
    On a GNU/Linux system, if installed in a system prefix:
     sudo ldconfig # as root

### Microsoft Windows

If you use Microsoft Visual C++ (7.1 or later) compiler, you can build
GEOS using NMAKE program and provided `makefile.vc` files.

If you are building from SVN or GIT checkout, first run: `autogen.bat`
Then:

    nmake /f makefile.vc MSVC_VER=1400

where 1400 is version number of Visual C++ compiler, here Visual C++ 8.0
from Visual Studio 2005 (supported versions are 1300, 1310, 1400, 1500,
1600, 1700, 1800 and 1900).
The bootstrap.bat step is required to generate a couple of header files.

In order to build debug configuration of GEOS, additional flag `DEBUG=1`
is required:

    nmake /f makefile.vc MSVC_VER=1400 DEBUG=1


## Client applications

### Using the C interface (recommended)

GEOS promises long term stability of C API

The C library uses the C++ interface, but the C library follows
normal ABI-change-sensitive versioning, so programs that link only
against the C library should work without relinking when GEOS is upgraded.

To compile programs against the C lib (recommended):

    CFLAGS += `geos-config --cflags`
    LDFLAGS += `geos-config --ldflags` -lgeos_c
    #include <geos_c.h>

Example usage:

    capi/geostest.c contains basic usage examples.

### Using the C++ interface (no stability promise)

Developers who decide to use the C++ interface should be aware GEOS
does not promise API or ABI stability of C++ API between releases.
Moreover C++ API/ABI breaking changes may not even be announced
or include in the NEWS file

The C++ library name will change on every minor release because
it is too hard to know if there have been ABI changes.

To compile programs against the C++ lib:

    CFLAGS += `geos-config --cflags`
    LDFLAGS += `geos-config --ldflags` -lgeos
    #include <geos.h>

CFLAGS, LDFLAGS这两个是make的隐含参数，在这里的目的是设置编译时的incluede目录和link lib目录和库名称。

其实`geos-config --cflags`是make并install geos生成的命令, 可以直接命令行运行，然后得到geos安装后的include目录，`geos-config --ldflags`得到的是link lib目录

至于geos库的名字，可以在link lib目录下查看，在ubuntu下编译geos得到的库的名字是libgeos.a, libgeos.so, libgeos_c.a, libgeos_c.so.



Basic usage examples can be found in `doc/example.cpp`.

See details in test-geos-example/.


### Scripting language bindings

#### Ruby
Ruby bindings are fully supported. To build, use the `--enable-ruby` option
when configuring:

    ./configure .. --enable-ruby

#### PHP
Since version 3.6.0 PHP bindings are not included in the core
library anymore but available as a separate project:

* https://git.osgeo.org/gitea/geos/php-geos

#### Python
Since version 3.0, the Python bindings are unsupported. Recommended options:

 1. Become or recruit a new maintainer.
 2. Use [Shapely](http://pypi.python.org/pypi/Shapely) with Python
    versions 2.4 or greater.
 3. Call functions from `libgeos_c` via Python ctypes.

## Documentation

To build Doxygen documentation:

    cd doc
    make doxygen-html

只有autotool 支持，cmake暂不支持，包括doc/example.cpp的编译

[#41 making cmake work also on doc directory](https://git.osgeo.org/gitea/geos/geos/pulls/41), 支持cmake编译

## Style

To format your code into the desired style, use astyle 3.1:

    astyle --style=stroustrup \
           --pad-comma \
           --indent=spaces=4 \
           --max-code-length=120 \
           --lineend=linux \
           yourfile.cpp

