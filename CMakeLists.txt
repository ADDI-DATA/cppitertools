cmake_minimum_required(VERSION 3.12)
project(cppitertools VERSION 2.1)

# define a header-only library
add_library(cppitertools INTERFACE)
add_library(cppitertools::cppitertools ALIAS cppitertools)

include(GNUInstallDirs)
target_include_directories(
  cppitertools INTERFACE $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
                         $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

# require C++17
target_compile_features(cppitertools INTERFACE cxx_std_17)

# Make package findable
configure_file("cmake/dummy-config.cmake.in" "cppitertools-config.cmake" @ONLY)

# Enable version checks in find_package
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  "${PROJECT_BINARY_DIR}/cppitertools-config-version.cmake"
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY SameMajorVersion)

# install and export target
install(TARGETS cppitertools EXPORT cppitertools-targets)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/cppitertools
        DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(
  EXPORT cppitertools-targets
  FILE "cppitertools-config.cmake"
  NAMESPACE cppitertools::
  DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/cppitertools/cmake)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/cppitertools-config-version.cmake
        DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/cppitertools/cmake)
