# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.0] - 2020-09-29

### Changed

* Rename public header to `sndwave.h` and move to `sndwave/` directory
* Rename `sndfile` library name to `sndwave`
* Rename `sndfile.pc` to `sndwave.pc`
* Rename program names from `sndfile-XXX` to `sndwave-XXX`
* Rename manpage names from `sndfile-XXX` to `sndwave-XXX`
* Rename CMake library name from `sndfile` to `sndwave`
* Rename CMake library exported target from `SndFile::sndfile` to `SndWave::sndwave`
* Bump required CMake version up to 3.15
* Change public functions export method, use compiler pragmas (in `sndwave/sndwave_export.h`)
  instead of symbol definition files
* MinGW and Cygwin DLLs now have Autotools compliant `(lib|cyg)sndfile-1.dll` name
  instead of `(lib|cyg)sndfile.dll` to fix known CMake bug
* Updated README.md and some other documentation files

### Removed

* [AutoGen](https://www.gnu.org/software/autogen/) requirement to build tests
* Python requirement for shared library
* Autotools build system and related scripts
* Existing documentation from `doc/` directory
* Example from `Win32/` directory
* Some other unuseful files
* CMake's option `ENABLE_COMPATIBLE_LIBSNDFILE_NAME` (useless for `libsndwave`)
* CMake's option `ENABLE_STATIC_RUNTIME` (we require CMAKE >= 3.15)

[unreleased]: https://github.com/evpobr/libsndwave/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/evpobr/libsndwave/releases/tag/v1.0.0
