[![Build Status](https://travis-ci.org/geotrellis/gdal-warp-bindings.svg?branch=master)](https://travis-ci.org/geotrellis/gdal-warp-bindings) [![Download](https://api.bintray.com/packages/azavea/geotrellis/gdal-warp-bindings/images/download.svg)](https://bintray.com/azavea/geotrellis/gdal-warp-bindings/_latestVersion)

# Intro #

Having multi-threaded access to raster data is an important prerequisite to constructing a high-performance GIS tile server.
If one wishes to use [GDAL's VRT functionality](https://www.gdal.org/gdal_vrttut.html) in this way, then it is necessary to ensure that no VRT dataset is simultaneously used by more than one thread, even for read-only operations.
The code in this repository attempts to address that issue by wrapping GDAL datasets in objects which abstract one or more identical datasets; the wrapped datasets can be safely used from multiple threads with less contention than would be the case with a simple mutex around one GDAL dataset.
APIs are provided for C and Java.

# Repository Structure #

The [`Docker`](Docker) directory contains files used to generate images used for continuous integration testing and deployment.
The [`src`](src) directory contains all of the source code for the library; the C/C++ files are in that directory.
[`src/main`](src/main) contains the code for the Java API.
[`src/unit_tests`](src/unit_tests) contains the C++ unit tests.

# Ports #

The repository contains [everything needed](Docker/Dockerfile.environment) to compile Linux, Macintosh, and Windows versions of the library in a Docker container.

## How to Build ##

All three can easily be compiled in the normal manner outside of a container if all dependencies are present.

### Linux ###

If a recent [GDAL](https://www.gdal.org/) and recent [JDK](https://openjdk.java.net/) are installed, the Linux version can be built by typing `make -C src` from the root directory of the cloned repository.
If one only wishes to use the C/C++ library, then type `make -C src libgdalwarp_bindings.so`.

### Macintosh ###

If a recent GDAL is installed (perhaps from Homebrew) and a recent JDK is installed, the Macintosh version can be built on a Macintosh by typing `OS=darwin SO=dylib make -C src` from the root directory of the cloned repository.
If one only wishes to use the C/C++ library, then type `OS=darwin SO=dylib make -C src libgdalwarp_bindings.dylib`.

### Windows ###

The Windows version has only been cross-built with [MinGW](http://www.mingw.org/wiki/InstallationHOWTOforMinGW) from within a Linux Docker container.
Please see the [continuous integration script](.travis/tests.sh) for more.

# Binary Artifacts #

The binary artifacts are present on [Bintray](https://bintray.com/azavea/geotrellis/gdal-warp-bindings/_latestVersion).
This jar file contains Linux, Macintosh, and Windows shared libraries.
All native binaries are for AMD64; the Linux ones are linked against GDAL 2.4.0, The Macintosh ones are linked against GDAL 2.4.0 from Homebrew, and the Windows ones are linked against the GDAL 2.4.0 MSVC 2015 build from [GISinternals.com](http://www.gisinternals.com/release.php).

The class files in the jar were built with OpenJDK 8.

The jar file is reachable via Maven:
```
<dependency>
  <groupId>com.azavea.gdal</groupId>
  <artifactId>gdal-warp-bindings</artifactId>
  <version>33.xxxxxxx</version>
  <type>pom</type>
</dependency>
```
