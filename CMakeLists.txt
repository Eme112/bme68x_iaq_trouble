#
# Copyright (c) 2023 Nordic Semiconductor
#
# SPDX-License-Identifier: LicenseRef-Nordic-5-Clause
#

cmake_minimum_required(VERSION 3.20.0)

find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})
project(bme68x_iaq)

FILE(GLOB app_sources src/*.c)
# NORDIC SDK APP START
target_sources(app PRIVATE ${app_sources})
# NORDIC SDK APP END

# Create a wrapper CMake library that our app can link with
add_library(mylibalgobsec STATIC IMPORTED GLOBAL)
set_target_properties(mylibalgobsec PROPERTIES IMPORTED_LOCATION ${BSEC_DIR}/BSEC/algo/bsec_IAQ/bin/gcc/Cortex_M4/libalgobsec.a)
set_target_properties(mylibalgobsec PROPERTIES INTERFACE_INCLUDE_DIRECTORIES /opt/nordic/ncs/v2.7.0/modules/lib/bme68x/src/bme68x)

# # Ensure the floating-point ABI matches
# set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mfloat-abi=soft")
# set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -mfloat-abi=soft")

target_link_libraries(app PUBLIC mylibalgobsec)