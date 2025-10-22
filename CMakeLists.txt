cmake_minimum_required(VERSION 3.10)
project(NodeJS_Dependencies)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(NODE_ROOT "${CMAKE_CURRENT_SOURCE_DIR}")

set(NODE_INCLUDE_DIR "${NODE_ROOT}/src")
set(V8_INCLUDE_DIR "${NODE_ROOT}/deps/v8/include")
set(UV_INCLUDE_DIR "${NODE_ROOT}/deps/uv/include")

if(WIN32)
    message(STATUS "Configuring NodeJS dependencies for Windows")
    if(CMAKE_BUILD_TYPE STREQUAL "Debug")
        set(NODE_LIB_DIR "${NODE_ROOT}/Win/bin/Debug")
        message(STATUS "Using Debug libraries from ${NODE_LIB_DIR}")
    else()
        set(NODE_LIB_DIR "${NODE_ROOT}/Win/bin/Release")
        message(STATUS "Using Release libraries from ${NODE_LIB_DIR}")
    endif()
    set(NODE_DEPS_LIB_DIR "${NODE_ROOT}/Win/bin/lib")
    set(NODE_LIBRARY_NAME "libnode.lib")

elseif(UNIX AND NOT APPLE)
    message(STATUS "Configuring NodeJS dependencies for Linux")
    if(CMAKE_BUILD_TYPE STREQUAL "Debug")
        set(NODE_LIB_DIR "${NODE_ROOT}/Linux/lib/Debug")
        message(STATUS "Using Debug libraries from ${NODE_LIB_DIR}")
    else()
        set(NODE_LIB_DIR "${NODE_ROOT}/Linux/lib/Release")
        message(STATUS "Using Release libraries from ${NODE_LIB_DIR}")
    endif()
    set(NODE_DEPS_LIB_DIR "")
    set(NODE_LIBRARY_NAME "node")

else()
    message(FATAL_ERROR "Unsupported platform for NodeJS dependencies")
endif()

add_library(nodejs_deps INTERFACE)

target_include_directories(nodejs_deps INTERFACE
        ${NODE_INCLUDE_DIR}
        ${V8_INCLUDE_DIR}
        ${UV_INCLUDE_DIR}
)

target_link_directories(nodejs_deps INTERFACE
        ${NODE_LIB_DIR}
)
if(NODE_DEPS_LIB_DIR)
    target_link_directories(nodejs_deps INTERFACE ${NODE_DEPS_LIB_DIR})
endif()
target_link_libraries(nodejs_deps INTERFACE
        ${NODE_LIBRARY_NAME}
)

set(V8_INCLUDE_DIR ${V8_INCLUDE_DIR} PARENT_SCOPE)
set(UV_INCLUDE_DIR ${UV_INCLUDE_DIR} PARENT_SCOPE)
set(NODE_INCLUDE_DIR ${NODE_INCLUDE_DIR} PARENT_SCOPE)
set(NODE_LIB_DIR ${NODE_LIB_DIR} PARENT_SCOPE)
if(NODE_DEPS_LIB_DIR)
    set(NODE_DEPS_LIB_DIR ${NODE_DEPS_LIB_DIR} PARENT_SCOPE)
endif()
set(NODEJS_DEPS_TARGET nodejs_deps PARENT_SCOPE)