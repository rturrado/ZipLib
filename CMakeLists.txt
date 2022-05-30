cmake_minimum_required(VERSION 3.20)

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

find_package(bzip2 REQUIRED)
find_package(liblzma REQUIRED)
find_package(zlib REQUIRED)

file(GLOB CPP_FILES *.cpp)
file(GLOB DETAIL_FILES detail/*.cpp)

add_library(${PROJECT_NAME} STATIC 
    ${CPP_FILES}
    ${DETAIL_FILES}
)

target_link_libraries(${PROJECT_NAME} PRIVATE bzip2 liblzma::liblzma zlib Threads::Threads)
target_include_directories (${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_20)
