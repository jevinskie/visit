cmake_minimum_required(VERSION 3.13)
project(visit)

set(CMAKE_CXX_STANDARD 17)

if (NOT TARGET Catch2 AND BUILD_TESTING)
    add_subdirectory(catch2)
endif()

if (NOT TARGET benchmark AND ENABLE_BENCHMARKS)
    set(BENCHMARK_ENABLE_TESTING false)
    add_subdirectory(gbench)
endif()


set(VISIT_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include)

add_library(visit INTERFACE)
target_include_directories(visit INTERFACE $<BUILD_INTERFACE:${VISIT_INCLUDE_DIR}>)
target_include_directories(visit INTERFACE $<INSTALL_INTERFACE:$<INSTALL_PREFIX>/include>)

if (BUILD_TESTING)
add_executable(visit_test main.cpp include/visit.hpp tests.cpp)
target_link_libraries(visit_test PRIVATE visit Catch2::Catch2)
endif()

if (ENABLE_BENCHMARKS)
add_executable(visit_benchmark benchmark.cpp include/visit.hpp)
target_link_libraries(visit_benchmark PRIVATE visit benchmark benchmark_main)
endif()
