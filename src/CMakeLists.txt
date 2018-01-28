cmake_minimum_required(VERSION 2.8.11)
project(lcm-spy-lite)

# cmake module path
list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/modules")

# automatically build LCM types.  This also defines a number of CMake
# variables, see cmake/lcmtypes.cmake for details
include(cmake/lcmtypes.cmake)
lcmtypes_build()

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu99")

## Config common include directories
include_directories(${CMAKE_SOURCE_DIR})

## Find LCM
find_package(LCM REQUIRED)
if(LCM_NOT_FOUND)
  message(STATUS "LCM is not found. Inter-process communication depends on LCM.")
endif(LCM_NOT_FOUND)
# Include files for LCM
set(LCM_GENERATED_HEADER_PATH ${CMAKE_BINARY_DIR}/include)
include_directories(${LCM_GENERATED_HEADER_PATH})

find_package(GLIB REQUIRED)
include_directories(${GLIB2_INCLUDE_DIRS})

## Add other sub source directories
add_subdirectory(apps)
add_subdirectory(spylite)

set(LCMTYPE_LIB_SRC
	lcmtypes/librav_lcm_msgs_BalCommand_t.c
	lcmtypes/librav_lcm_msgs_BalRawIMU_t.c
	lcmtypes/librav_lcm_msgs_BalRawSpeed_t.c
)
add_library(bal_lcmtypes SHARED ${LCMTYPE_LIB_SRC})
# target_link_libraries(bal_lcmtypes ${CMAKE_THREAD_LIBS_INIT} ${LCM_LIBRARIES} ${GLIB2_LIBRARIES} -ldl)
# target_include_directories(bal_lcmtypes PUBLIC
# 	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
# 	$<INSTALL_INTERFACE:include>)