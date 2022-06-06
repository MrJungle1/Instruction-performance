cmake_minimum_required(VERSION 3.15.2)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON CACHE INTERNAL "")

project(MegEngine LANGUAGES C CXX ASM)

message(${PROJECT_SOURCE_DIR})
message(${CMAKE_CURRENT_SOURCE_DIR})

option(MEGPEAK_ENABLE_OPENCL "Build MegPeak with OpenCL." OFF)

set(MGEPEAK_ARCH "aarch64")
# set(MGEPEAK_ARCH "armv7")
# set(MGEPEAK_ARCH "x86")

message(STATUS "CONFIG MGEPEAK_ARCH TO ${MGEPEAK_ARCH}")

file(GLOB_RECURSE SRC src/*.cpp src/*.h )

add_executable(megpeak ${SRC})
target_include_directories(megpeak PUBLIC ${PROJECT_SOURCE_DIR})

set(CMAKE_CXX_FLAGS "-Ofast -g ${CMAKE_CXX_FLAGS}")
if(${MGEPEAK_ARCH} STREQUAL "aarch64")
    set(CMAKE_CXX_FLAGS " -DMEGPEAK_AARCH64=1 ${CMAKE_CXX_FLAGS}")
elseif(${MGEPEAK_ARCH} STREQUAL "armv7")
    set(CMAKE_CXX_FLAGS " -DMEGPEAK_ARMV7=1 ${CMAKE_CXX_FLAGS}")
elseif(${MGEPEAK_ARCH} STREQUAL "x86")
    set(CMAKE_CXX_FLAGS " -DMEGPEAK_X86=1 ${CMAKE_CXX_FLAGS}")
else()
    message(FATAL_ERROR "Unknown MEGPEAK_ARCH ${MEGPEAK_ARCH}.")
endif()

if(MEGPEAK_ENABLE_OPENCL)
    set(CMAKE_CXX_FLAGS " -DMEGPEAK_WITH_OPENCL=1 ${CMAKE_CXX_FLAGS}")

    message(STATUS "megpeak build with opencl.")
    add_subdirectory(opencl-stub)
    target_include_directories(megpeak PUBLIC ${PROJECT_SOURCE_DIR}/opencl-stub/include/)
    target_link_libraries(megpeak OpenCL)
endif()

if(UNIX)
    target_link_libraries(megpeak dl)
endif()
if(ANDROID)
    target_link_libraries(megpeak log)
endif()
