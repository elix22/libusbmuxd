cmake_minimum_required(VERSION 3.0)
project(libusbmuxd)


set(CMAKE_CXX_STANDARD 20)

if(CMAKE_SYSTEM_NAME STREQUAL "Windows")
    add_definitions(-DHAVE_STRNDUP -DHAVE_STPNCPY -DPACKAGE_VERSION="v1.3" -DPACKAGE_STRING="v1.3")
elseif(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    add_definitions(-DHAVE_STRNDUP -DHAVE_STPNCPY -DHAVE_GETIFADDRS -DPACKAGE_VERSION="v1.3" -DPACKAGE_STRING="v1.3")
elseif(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    add_definitions(-DHAVE_STRNDUP -DHAVE_STPNCPY -DHAVE_GETIFADDRS -DPACKAGE_VERSION="v1.3" -DPACKAGE_STRING="v1.3")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++ -x objective-c++ ")
endif()

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../libimobiledevice-glue ${CMAKE_CURRENT_SOURCE_DIR}/../libimobiledevice-glue/build)

# Collect all .cpp files recursively
file(GLOB_RECURSE LIBUSBMUXD_SOURCES  src/*.c src/*.h)

add_library(libusbmuxd SHARED ${LIBUSBMUXD_SOURCES})

target_include_directories(libusbmuxd PRIVATE include src ../libplist/include ../libimobiledevice-glue/include)
# Link libplist
target_link_libraries(libusbmuxd PRIVATE libimobiledevice-glue libplist)
