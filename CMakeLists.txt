# Edit cmake/custom.cmake with overrides for the variables set in cmake/defaults.cmake.
# Call cmake defining the variables you need to build the modules you want and set -G "MSYS Makefile".
# Important variables are
# GNC_BUILD_GLIB Specifies GLib, Harfbuzz, and their dependencies.
# GNC_BUILD_DEPS Specifies all dependencies.
# "Specified" dependencies will be built into ${GLOBAL_DEP_BUILD_DIR}.
# GNC_INSTALL_DEPS Installs the dependencies into ${GLOBAL_DIR}.
# GNC_MAKE_TARBALLS Makes the distribution tarballs for upload to SourceForge from the specified dependencies.
cmake_minimum_required (VERSION 3.3.2)
include (cmake/defaults.cmake)
include (cmake/custom.cmake)
include (ExternalProject)
# We can't set up any environment here so we must call make with some
# enviroment variables set on the command line. Note that Python >=
# 2.5 is required to install glib in order to create
# glib-codegen. It's not needed, but that doesn't seem to matter.
# PATH=$PATH:<INSTALL_DIR>/bin:<INSTALL_DIR>/lib PKG_CONFIG_PATH=<INSTALL_DIR>/lib/pkgconfig CXXFLAGS="$CXXFLAGS -std=gnu++11" PYTHON=/path/to/python.exe make
# Note especially that -std=g++11; if it's not set then icu will set
# it to -std=c++0x which breaks building harfbuzz because it uses
# strdup which is a GCC extention.
if (GNC_BUILD_DEPS OR GNC_BUILD_GLIB)
include (cmake/glib.cmake)
endif()
