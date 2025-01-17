cmake_minimum_required(VERSION 3.16)

project(httpclient)

set(CMAKE_CXX_STANDARD 20)

message(STATUS "CMAKE_SYSTEM_NAME: ${CMAKE_SYSTEM_NAME}")

if(${CMAKE_SYSTEM_NAME} STREQUAL "WASI")
    set(CMAKE_SHARED_LIBRARY_PREFIX "")
    set(CMAKE_SHARED_LIBRARY_SUFFIX ".wasm")
    set(CMAKE_EXECUTABLE_SUFFIX ".wasm")

    include_directories(${CMAKE_SOURCE_DIR}/wasmedge_wasi_socket_c/include) 
    add_subdirectory(wasmedge_wasi_socket_c)
endif()

add_subdirectory(libfetch)

add_executable(
   http_get
   http_get.cpp
)

target_link_libraries(
    http_get
    fetch
)

if("${CMAKE_SYSTEM_NAME}" STREQUAL "WASI")
    target_link_libraries(
        fetch
        wasmedge_wasi_socket_c
    )
endif()
