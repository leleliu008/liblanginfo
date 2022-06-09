cmake_minimum_required(VERSION 3.0.0)

project(liblanginfo CXX)

set(PROJECT_VERSION       1.0.0)
set(PROJECT_VERSION_MAJOR 1)
set(PROJECT_VERSION_MINOR 0)
set(PROJECT_VERSION_PATCH 0)

if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()
########################################################################################

add_library(liblanginfo-static STATIC langinfo.cpp)
add_library(liblanginfo-shared SHARED langinfo.cpp)

set_target_properties(liblanginfo-static PROPERTIES OUTPUT_NAME langinfo CLEAN_DIRECT_OUTPUT 1 EXPORT_NAME static)
set_target_properties(liblanginfo-shared PROPERTIES OUTPUT_NAME langinfo CLEAN_DIRECT_OUTPUT 1 EXPORT_NAME shared)

########################################################################################

include(GNUInstallDirs)
install(FILES langinfo.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

########################################################################################

configure_file(liblanginfo.pc.in liblanginfo.pc @ONLY)
install(
    FILES       "${CMAKE_CURRENT_BINARY_DIR}/liblanginfo.pc"
    DESTINATION "${CMAKE_INSTALL_LIBDIR}/pkgconfig"
)

########################################################################################

set(CMAKE_INSTALL_CONFIGDIR "${CMAKE_INSTALL_LIBDIR}/cmake/liblanginfo")

install(
    TARGETS     liblanginfo-static
    EXPORT      liblanginfo-staticTargets
    ARCHIVE
    DESTINATION "${CMAKE_INSTALL_LIBDIR}"
)
install(
    EXPORT      liblanginfo-staticTargets
    FILE        liblanginfo-staticTargets.cmake
    NAMESPACE   liblanginfo::
    DESTINATION "${CMAKE_INSTALL_CONFIGDIR}"
)

install(
    TARGETS     liblanginfo-shared
    EXPORT      liblanginfo-sharedTargets
    LIBRARY
    DESTINATION "${CMAKE_INSTALL_LIBDIR}"
)
install(
    EXPORT      liblanginfo-sharedTargets
    FILE        liblanginfo-sharedTargets.cmake
    NAMESPACE   liblanginfo::
    DESTINATION "${CMAKE_INSTALL_CONFIGDIR}"
)

########################################################################################

include(CMakePackageConfigHelpers)

write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/liblanginfoConfigVersion.cmake"
    VERSION "${PROJECT_VERSION}"
    COMPATIBILITY SameMajorVersion
)
install(
    FILES       "${CMAKE_CURRENT_BINARY_DIR}/liblanginfoConfigVersion.cmake"
    DESTINATION "${CMAKE_INSTALL_CONFIGDIR}")

configure_package_config_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/liblanginfoConfig.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/liblanginfoConfig.cmake"
    INSTALL_DESTINATION  "${CMAKE_INSTALL_CONFIGDIR}"
)
install(
    FILES "${CMAKE_CURRENT_BINARY_DIR}/liblanginfoConfig.cmake"
    DESTINATION "${CMAKE_INSTALL_CONFIGDIR}"
)
