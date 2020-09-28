# libsndwave

![C/C++ CI](https://github.com/evpobr/libsndwave/workflows/C/C++%20CI/badge.svg)

A library for reading and writing audio files.

## Hacking

The canonical source code repository for libsndwave is at
[https://github.com/evpobr/libsndwave/][github].

You can grab the source code using:

    git clone git://github.com/evpobr/libsndwave.git

Setting up a build environment for libsndwave on Debian or Ubuntu is as simple as:

    sudo apt install build-essential cmake libasound2-dev libflac-dev libogg-dev \
        libvorbis-dev libopus-dev pkg-config

For other Linux distributions or any of the *BSDs, the setup should be similar
although the package install tools and package names may be slightly different.

Similarly on Mac OS X, assuming [brew] is already installed:

    brew install cmake flac libogg libtool libvorbis opus pkg-config


## The CMake build system

The build process with CMake takes place in two stages. First, standard build files
are created from configuration scripts. Then the platform's native build tools are
used for the actual building. CMake can produce Microsoft Visual Studio project
and solution files, Unix Makefiles, Xcode projects and [many more](https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html).

Some IDE support CMake natively or with plugins, check you IDE documentation
 for details.

### Requirements

1. C99-compliant compiler toolchain (tested with GCC, Clang and Visual
   Studio 2015)
2. CMake 3.13 or newer

There are some recommended packages to enable all features of libsndwave:

1. Ogg, Vorbis and FLAC libraries and headers to enable these formats support
2. ALSA development package under Linux to build sndwave-play utility
3. Sndio development package under BSD to build sndwave-play utility

### Building from command line

CMake can handle out-of-place builds, enabling several builds from
the same source tree, and cross-compilation. The ability to build a directory
tree outside the source tree is a key feature, ensuring that if a build
directory is removed, the source files remain unaffected.

    mkdir CMakeBuild
    cd CMakeBuild

Then run `cmake` command with directory where CMakeLists.txt script is located
as argument (relative paths are supported):

    cmake ..

This command will configure and write build script or solution to CMakeBuild
directory. CMake is smart enough to create Unix makefiles under Linux or Visual
Studio solution if you have Visual Studio installed, but you can configure
[generator](https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html)
with `-G` command line parameter:

    cmake .. -G"Unix Makefiles"

The build procedure depends on the selected generator. With "Unix Makefiles" you
can type:

    make & make install

With "Visual Studio" and some other generators you can open solution or project
from `CMakeBuild` directory and build using IDE.

Finally, you can use unified command:

    cmake --build .

CMake also provides Qt-based cross platform GUI, cmake-gui. Using it is trivial
and does not require detailed explanations.

### Configuring CMake

You can pass additional options with `/D<parameter>=<value>` when you run
`cmake` command. Some useful system options:

* `CMAKE_C_FLAGS` - additional C compiler flags
* `CMAKE_BUILD_TYPE` - configuration type, `DEBUG`, `RELEASE`, `RELWITHDEBINFO`
  or `MINSIZEREL`. `DEBUG` is default
* `CMAKE_INSTALL_PREFIX` - build install location, the same as `--prefix` option
  of `configure` script

 Useful libsndwave options:

* `BUILD_SHARED_LIBS` - build shared library (DLL under Windows) when `ON`,
  build static library othervise. This option is `OFF` by default.
* `BUILD_PROGRAMS` - build libsndwave's utilities from `programs/` directory,
  `ON` by default.
* `BUILD_EXAMPLES` - build examples, `ON` by default.
* `BUILD_TESTING` - build tests. Then you can run tests with `ctest` command,
  `ON` by default. Setting `BUILD_SHARED_LIBS` to `ON` disables this option.
* `ENABLE_EXTERNAL_LIBS` - enable Ogg, Vorbis, FLAC and Opus support. This
  option is available and set to `ON` if all dependency libraries were found.
* `ENABLE_CPU_CLIP` - enable tricky cpu specific clipper. Enabled and set to
  `ON` when CPU clips negative\positive. Don't touch it if you are not sure
* `ENABLE_BOW_DOCS` - enable black-on-white documentation theme, `OFF` by
  default.
* `ENABLE_EXPERIMENTAL` - enable experimental code. Don't use it if you are
  not sure. This option is `OFF` by default.
* `ENABLE_CPACK` - enable [CPack](https://cmake.org/cmake/help/latest/module/CPack.html) support.
  This option is `ON` by default.
* `ENABLE_PACKAGE_CONFIG` - generate and install [package config file](https://cmake.org/cmake/help/latest/manual/cmake-packages.7.html#config-file-packages).
* `INSTALL_PKGCONFIG_MODULE` - generate and install [pkg-config module](https://people.freedesktop.org/~dbn/pkg-config-guide.html).
* `INSTALL_MANPAGES` - install [man pages](https://en.wikipedia.org/wiki/Man_page) for programs. This option is `ON` by default
  on Unix, MinGW and Cygwin platforms
  
* `ENABLE_STATIC_RUNTIME` - enable static runtime on Windows platform, `OFF` by
  default (CMake < 3.15).

  **Note**: For MSVC compiler this option is deprecated and disabled for CMake >= 3.15, see
  policy [CMP0091](https://cmake.org/cmake/help/latest/policy/CMP0091.html).
  Use `CMAKE_MSVC_RUNTIME_LIBRARY` option instead.

Deprecated options:

* `DISABLE_EXTERNAL_LIBS` - disable Ogg, Vorbis and FLAC support. Replaced by
  `ENABLE_EXTERNAL_LIBS`
* `DISABLE_CPU_CLIP` - disable tricky cpu specific clipper. Replaced by
  `ENABLE_CPU_CLIP`
* `BUILD_STATIC_LIBS` - build static library. Use `BUILD_SHARED_LIBS` instead

### Linking from CMake projects

First you need to add `FindOgg.cmake`, `FindVorbis.cmake`, `FindFLAC.cmake` and
`FindOpus.cmake` files to some directory inside your CMake project (usually
`cmake`) and add it to `CMAKE_MODULE_PATH`:

    project(SomeApplication)
    
    list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

Now you can search `libsndwave` library from your `CMakeLists.txt`
 with this command:

    find_package(SndWave)

`SndWave_FOUND` is set to `ON` when library is found.

If `libsndwave` dependency is critical, you can add `REQUIRED` to
 `find_package`:

    find_package(SndWave REQUIRED)

With with option `find_package` will terminate configuration process
 if `libsndwave` is not found.

You can also add version check:

    find_package(SndWave 1.0.29)

`find_package` will report error, if `libsndwave` version is < 1.0.29.

You can combine `REQUIRED` and version if you need.

To link `libsndwave` library use:

    target_link_libraries(my_application PRIVATE SndWave::sndwave)

### Notes for Windows users

First advice - set `ENABLE_STATIC_RUNTIME` to ON. This will remove dependencies
on runtime DLLs.

Second advice is about Ogg, Vorbis FLAC and Opus support. Searching external
libraries under Windows is a little bit tricky. The best way is to use
[Vcpkg](https://github.com/Microsoft/vcpkg). You need to install static libogg,
libvorbis, libflac and libopus libraries:

    vcpkg install libogg:x64-windows-static libvorbis:x64-windows-static
    libflac:x64-windows-static opus:x64-windows-static libogg:x86-windows-static
    libvorbis:x86-windows-static libflac:x86-windows-static opus:x86-windows-static

Then and add this parameter to cmake command line:

    -DCMAKE_TOOLCHAIN_FILE=<path-to-vcpkg>/scripts/buildsystems/vcpkg.cmake

You also need to set `VCPKG_TARGET_TRIPLET` because you use static libraries:

    -DVCPKG_TARGET_TRIPLET=x64-windows-static

## Submitting Patches

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

[brew]: http://brew.sh/
[github]: https://github.com/evpobr/libsndwave/
