cmake_minimum_required(VERSION 2.8.10)

project(android_native_app_glue)

if(MINGW OR UNIX)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-attributes")
endif()

set(NATIVE_APP_GLUE_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/native_app_glue)
include_directories( ${NATIVE_APP_GLUE_SOURCE_DIR}/include)

if(NATIVE_APP_GLUE_BUILD_SHARED)
	set(NATIVE_APP_GLUE_LIBTYPE SHARED)
else()
	set(NATIVE_APP_GLUE_LIBTYPE STATIC)
endif()

set(API 
	${NATIVE_APP_GLUE_SOURCE_DIR}/include/android_native_app_glue.h)

source_group(API FILES ${API})

set(SOURCES
	${NATIVE_APP_GLUE_SOURCE_DIR}/src/android_native_app_glue.c)

add_library(android_native_app_glue ${NATIVE_APP_GLUE_LIBTYPE} ${SOURCES} ${API})

set_target_properties(android_native_app_glue PROPERTIES PUBLIC_HEADER "${API}")

set(BINDIR bin)
set(BINCOMPONENT runtime)
set(LIBDIR lib)
set(LIBCOMPONENT dev)
set(INCLUDEDIR include)
set(INCLUDECOMPONENT dev)

install(TARGETS android_native_app_glue
	RUNTIME DESTINATION ${BINDIR} COMPONENT ${BINCOMPONENT}
	LIBRARY DESTINATION ${LIBDIR} COMPONENT ${LIBCOMPONENT}
	ARCHIVE DESTINATION ${LIBDIR} COMPONENT ${LIBCOMPONENT}
	PUBLIC_HEADER DESTINATION ${INCLUDEDIR} COMPONENT ${INCLUDECOMPONENT})

