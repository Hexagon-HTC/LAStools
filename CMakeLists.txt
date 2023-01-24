cmake_minimum_required(VERSION 2.8 FATAL_ERROR)
set(CMAKE_SUPPRESS_REGENERATION true)
project("LAStools")

if (!MSVC)
	add_compile_options(-O3 -Wall -Wno-strict-aliasing)
else()
	add_definitions(-D_CRT_SECURE_NO_WARNINGS)
endif()

option(BUILD_SHARED_LIBS "Build LASlib as DLL" OFF)

if (BUILD_SHARED_LIBS AND UNIX AND NOT APPLE)
	set(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib/LASlib")
endif()

add_subdirectory(LASlib/src)
add_subdirectory(src)
if (EXISTS src_full)
	add_subdirectory(src_full)
endif()

add_library(LAStools SHARED)

install(TARGETS LAStools
        EXPORT LAStools
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib/static
        PUBLIC_HEADER DESTINATION include/Lib)

install(EXPORT LAStools NAMESPACE LAStools:: DESTINATION lib/cmake/LAStools)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/LAStoolsConfig.cmake
    DESTINATION lib/cmake/LAStools)
