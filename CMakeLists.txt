cmake_minimum_required(VERSION 3.28)
project(libfzn CXX)

# C++
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Dependency
include(FetchContent)
FetchContent_Declare(
        peglib
        GIT_REPOSITORY https://github.com/yhirose/cpp-peglib.git
        GIT_TAG        master
)
set(BUILD_TESTS OFF CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(peglib)

# Sources
set(LIBFZN_SRC
    Parser.cpp
    Printer.cpp
)

# Library
add_library(libfzn ${LIBFZN_SRC})
set_target_properties(libfzn PROPERTIES PREFIX "")
target_link_libraries(libfzn PUBLIC peglib)
target_include_directories(libfzn PUBLIC ${PROJECT_SOURCE_DIR})

# Demo
if (PROJECT_IS_TOP_LEVEL)
    add_executable(libfzn-demo Demo.cpp)
    target_link_libraries(libfzn-demo PRIVATE libfzn)
endif()
