cmake_minimum_required(VERSION 3.10)
project(NodeJS_Dependencies)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(NODE_ROOT "${CMAKE_CURRENT_SOURCE_DIR}")
set(NODE_INCLUDE_DIR "${NODE_ROOT}/src")
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(NODE_LIB_DIR "${NODE_ROOT}/Win/bin/Debug")
    set(NODE_DEPS_LIB_DIR "${NODE_ROOT}/Win/bin/Debug/lib")
    message(STATUS "Using Debug libraries from ${NODE_LIB_DIR}")
else()
    set(NODE_LIB_DIR "${NODE_ROOT}/Win/bin/Release")
    set(NODE_DEPS_LIB_DIR "${NODE_ROOT}/Win/bin/Release/lib")
    message(STATUS "Using Release libraries from ${NODE_LIB_DIR}")
endif()
set(V8_INCLUDE_DIR "${NODE_ROOT}/deps/v8/include")
set(UV_INCLUDE_DIR "${NODE_ROOT}/deps/uv/include")

add_library(nodejs_deps INTERFACE)

target_include_directories(nodejs_deps INTERFACE
        ${NODE_INCLUDE_DIR}
        ${V8_INCLUDE_DIR}
        ${UV_INCLUDE_DIR}
)

target_link_directories(nodejs_deps INTERFACE
        ${NODE_LIB_DIR}
        ${NODE_DEPS_LIB_DIR}
)

target_link_libraries(nodejs_deps INTERFACE
        libnode.lib
)

set(NODE_LIB_DIR ${NODE_LIB_DIR} PARENT_SCOPE)
set(NODE_DEPS_LIB_DIR ${NODE_DEPS_LIB_DIR} PARENT_SCOPE)
set(NODEJS_DEPS_TARGET nodejs_deps PARENT_SCOPE)