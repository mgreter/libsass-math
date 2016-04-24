CMAKE_MINIMUM_REQUIRED (VERSION 2.6)

project(math CXX)

if (MINGW)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -static-libgcc")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -static-libgcc -static-libstdc++")
    set(CMAKE_SHARED_LIBRARY_LINK_C_FLAGS "${CMAKE_SHARED_LIBRARY_LINK_C_FLAGS} -static-libgcc")
    set(CMAKE_SHARED_LIBRARY_LINK_CXX_FLAGS "${CMAKE_SHARED_LIBRARY_LINK_CXX_FLAGS} -static-libgcc -static-libstdc++")
endif()

set(CMAKE_SHARED_LIBRARY_PREFIX "")
set(CMAKE_STATIC_LIBRARY_PREFIX "")
set(CMAKE_IMPORT_LIBRARY_PREFIX "")

set(LIBSASS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/.." CACHE FILEPATH "Path to libsass directory")
set(LIBSASS_LIBRARY_DIR "${LIBSASS_DIR}/lib" CACHE FILEPATH "Path to libsass libraries")
set(LIBSASS_INCLUDE_DIR "${LIBSASS_DIR}/include" CACHE FILEPATH "Path to libsass includes")

link_directories("${LIBSASS_LIBRARY_DIR}")
include_directories("${LIBSASS_INCLUDE_DIR}")
# include_directories("vendor")

find_library(SASS_LIBRARY NAMES sass
    HINTS "${CMAKE_CURRENT_SOURCE_DIR}/../lib"
)

add_library(math SHARED src/math.cpp)
target_include_directories(math PUBLIC)
target_link_libraries(math LINK_PUBLIC sass)

# errors wrongly sometimes?
# if(NOT SASS_LIBRARY)
# 	message( FATAL_ERROR "sass library was not found" )
# endif()
