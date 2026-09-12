cmake_minimum_required(VERSION 3.15)
project(pymoku LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

include(FetchContent)
set(BUILD_SHARED_LIBS OFF CACHE BOOL "" FORCE)
FetchContent_Declare(
    moku
    GIT_REPOSITORY https://github.com/gn0rrbitberrenbur/moku.git
    GIT_TAG        v0.0.5
    GIT_SHALLOW    TRUE
)
FetchContent_MakeAvailable(moku)

find_package(pybind11 CONFIG REQUIRED)
find_package(Threads REQUIRED)

pybind11_add_module(_core
    src/bindings.cpp
    ${moku_SOURCE_DIR}/src/benchmark.cpp
)

target_link_libraries(_core PRIVATE moku_core Threads::Threads)
target_include_directories(_core PRIVATE ${moku_SOURCE_DIR}/include)

install(TARGETS _core LIBRARY DESTINATION pymoku)