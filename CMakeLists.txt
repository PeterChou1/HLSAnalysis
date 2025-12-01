cmake_minimum_required(VERSION 3.20)
project(hls LANGUAGES CXX C)

set(CMAKE_CXX_STANDARD 17 CACHE STRING "C++ standard")
set(LLVM_DIR ${CMAKE_SOURCE_DIR}/circt/llvm/build/lib/cmake/llvm)
set(MLIR_DIR ${CMAKE_SOURCE_DIR}/circt/llvm/build/lib/cmake/mlir)
find_package(LLVM REQUIRED CONFIG)
find_package(MLIR REQUIRED CONFIG)

message(STATUS "Found LLVM ${LLVM_PACKAGE_VERSION}")
message(STATUS "Using LLVMConfig.cmake in ${LLVM_DIR}")

list(APPEND CMAKE_MODULE_PATH "${MLIR_CMAKE_DIR}")
include(TableGen)
include(AddMLIR)
include(HandleLLVMOptions)

set(NO_RTTI "-fno-rtti")
add_definitions(${LLVM_DEFINITIONS} ${NO_RTTI})

include_directories(
    ${LLVM_INCLUDE_DIRS}
    ${MLIR_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

add_executable(hls
    src/AST.cpp
    src/main.cpp
)

target_link_libraries(hls
    PRIVATE
    MLIRSupport
    LLVMSupport
    LLVMOption
    LLVMRemarks
)
