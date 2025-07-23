cmake_minimum_required(VERSION 3.10.0)

project(ILight LANGUAGES CXX)

option(USE_BUNDLED_MINHOOK "Use the bundled MinHook submodule; if OFF, assumes MinHook is provided by the user" ON)
option(EXPOSE_MINHOOK "Make MinHook a public dependency of this project" ON)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(INC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/inc)
set(SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)

if (USE_BUNDLED_MINHOOK)
    message(STATUS "Using ${CMAKE_CURRENT_SOURCE_DIR}/deps/minhook")
    add_subdirectory(deps/minhook)
else()
    message(STATUS "Using user-provided minhook")
endif()

add_library(
    ${PROJECT_NAME}

    ${SRC_DIR}/debug/klass.cc

    ${SRC_DIR}/assemblies.cc
    ${SRC_DIR}/il2cpp.cc
    ${SRC_DIR}/state.cc
)

target_include_directories(${PROJECT_NAME} PUBLIC ${INC_DIR})

if(LINK_MINHOOK_PUBLIC)
    target_link_libraries(${PROJECT_NAME} PUBLIC minhook)
else()
    target_link_libraries(${PROJECT_NAME} PRIVATE minhook)
endif()