cmake_minimum_required(VERSION 3.20)

if (POLICY CMP0128)
    cmake_policy(SET CMP0128 NEW)
endif()

project(ZIPLIB
    LANGUAGES C CXX
)

if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    set(CMAKE_THREAD_PREFER_PTHREAD TRUE)
    set(THREADS_PREFER_PTHREAD_FLAG TRUE)
    add_compile_options("-lstdc++fs")
    set(CMAKE_CXX_COMPILER g++-11)
endif ()
find_package(Threads REQUIRED)

if(NOT ${CMAKE_BUILD_TYPE} STREQUAL "Debug")
    add_compile_options("-O2")
endif()

file(GLOB CPP_FILES *.cpp)
file(GLOB DETAIL_FILES detail/*.cpp)
file(GLOB BZIP2_FILES extlibs/bzip2/*.c)
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    file(GLOB LZMA_FILES extlibs/lzma/unix/*.c)
    file(GLOB LZMA_UTIL_FILES extlibs/lzma/unix/LzmaUtil/*.c)
else()
    file(GLOB LZMA_FILES extlibs/lzma/*.c)
endif()
file(GLOB ZLIB_FILES extlibs/zlib/*.c)

add_library(${PROJECT_NAME} STATIC 
    ${CPP_FILES}
    ${DETAIL_FILES}
    ${BZIP2_FILES}
    ${LZMA_FILES}
    ${LZMA_UTIL_FILES}
    ${ZLIB_FILES}
)

target_link_libraries(${PROJECT_NAME} Threads::Threads)
target_include_directories (${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_20)
