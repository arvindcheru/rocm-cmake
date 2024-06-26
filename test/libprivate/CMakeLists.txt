################################################################################
# Copyright (C) 2024 Advanced Micro Devices, Inc.
################################################################################


cmake_minimum_required (VERSION 3.5)
project(libprivate CXX)

find_package(ROCmCMakeBuildTools)

include(ROCMInstallTargets)
include(ROCMPackageConfigHelpers)
include(ROCMSetupVersion)
include(ROCMInstallSymlinks)
include(ROCMCreatePackage)

rocm_setup_version(VERSION 1.0.0)

if(ROCM_PREFIX)
rocm_create_package(
    NAME simple-private
    PREFIX ${ROCM_PREFIX}
    MAINTAINER "Amd amd@amd.com"
    PTH
    LDCONFIG)
else()
rocm_create_package(
    NAME simple-private
    MAINTAINER "Amd amd@amd.com"
    PTH
    LDCONFIG)
endif()

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/bin)

add_library(simple_private simpleprivate.cpp)
#rocm_set_soversion(simple-private 1.1.2)

add_executable(simple_private_main main.cpp)
target_link_libraries(simple_private_main simple_private)

if(ROCM_PREFIX)
    rocm_install_targets(PRIVATE TARGETS simple_private simple_private_main INCLUDE include PREFIX ${ROCM_PREFIX})
    rocm_export_targets(TARGETS simple_private INCLUDE ${CMAKE_CURRENT_SOURCE_DIR}/importtarget.cmake PREFIX ${ROCM_PREFIX})
    rocm_install_symlink_subdir(${ROCM_PREFIX})
else()
    rocm_install_targets(PRIVATE TARGETS simple_private simple_private_main INCLUDE include)
    rocm_export_targets(TARGETS simple_private INCLUDE ${CMAKE_CURRENT_SOURCE_DIR}/importtarget.cmake)
endif()
