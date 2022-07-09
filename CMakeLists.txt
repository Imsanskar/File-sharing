cmake_minimum_required(VERSION 3.20)



project(FileSharing LANGUAGES C CXX)

set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set (SRC "platform/src")
set (INCLUDE "platform/include")

include_directories(${INCLUDE})

set(CMAKE_BINARY_DIR ${CMAKE_SOURCE_DIR}/binaries)
if ( ${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_BINARY_DIR} )
    message( FATAL_ERROR "In-source builds not allowed. Please make a new directory (called a build directory) and run CMake from there. You may need to remove CMakeCache.txt." )
endif()

if(WIN32)
	add_executable(file_sharing ${SRC}/main.cpp)
	target_link_libraries(file_sharing gdi32 kernel32 user32)
	set (CMAKE_CXX_FLAGS "-std=c++20")
endif (WIN32)

if (UNIX)
    add_executable(file_sharing ${SRC}/main.cpp)
	target_link_libraries(file_sharing pthread dl  m)
	set (CMAKE_CXX_FLAGS "-std=c++20 -fsanitize=address")
	add_compile_definitions(_GLFW_X11)
endif (UNIX)
