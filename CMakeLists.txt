cmake_minimum_required(VERSION 2.8)
project(cubiomes)

if (NOT CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE Release)
endif()
if (WIN32)
	set(CMAKE_C_FLAGS "-D_WIN32")
endif()

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra")
set(CMAKE_C_FLAGS_DEBUG "-O0 -g3 -DDEBUG")

set(HEADERS
	cubiomes/finders.h
	cubiomes/generator.h
	cubiomes/layers.h
	cubiomes/biomenoise.h
	cubiomes/biomes.h
	cubiomes/noise.h
	cubiomes/rng.h
	cubiomes/util.h
	cubiomes/quadbase.h
)
set(SOURCES
	finders.c
	generator.c
	layers.c
	biomenoise.c
	biomes.c
	noise.c
	util.c
	quadbase.c
)

add_library(objects OBJECT ${SOURCES})
set_property(TARGET objects PROPERTY POSITION_INDEPENDENT_CODE 1)

# TODO: make include directories imprt as `#include <cubiomes/generator.h>` isntead of `#include <generator.h>`
#127 add include directories 
target_include_directories(cubiomes PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_include_directories(cubiomes_static PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

add_library(cubiomes SHARED $<TARGET_OBJECTS:objects>)
add_library(cubiomes_static STATIC $<TARGET_OBJECTS:objects>)

install(TARGETS cubiomes cubiomes_static DESTINATION lib)
install(FILES ${HEADERS} DESTINATION include)


