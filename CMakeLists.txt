# CMakeLists.txt Build files for GaloisCPP library
cmake_minimum_required(VERSION 3.5)
project(libGaloisCPP VERSION 1.0.0 LANGUAGES CXX)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

add_library(GaloisCPP
            src/galoisfield.cpp
            src/gfelement.cpp
            src/modular_arith.cpp
            src/polynomial_arith.cpp)

target_include_directories(
  GaloisCPP
  PUBLIC $<INSTALL_INTERFACE:include>
         $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>)

# Installation instructions
include(GNUInstallDirs)
set(INSTALL_CONFIGDIR ${CMAKE_INSTALL_LIBDIR}/cmake/GaloisCPP)
install(TARGETS GaloisCPP
        EXPORT GaloisCPP-targets
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})
install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

# Create a ConfigVersion.cmake file
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/GaloisCPPConfigVersion.cmake
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY AnyNewerVersion)

configure_package_config_file(
  ${CMAKE_CURRENT_LIST_DIR}/cmake/GaloisCPPConfig.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/GaloisCPPConfig.cmake
  INSTALL_DESTINATION
  ${INSTALL_CONFIGDIR})

# Install the config, configversion and custom find modules
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/GaloisCPPConfig.cmake
              ${CMAKE_CURRENT_BINARY_DIR}/GaloisCPPConfigVersion.cmake
        DESTINATION ${INSTALL_CONFIGDIR})
