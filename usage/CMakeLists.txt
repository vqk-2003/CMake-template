# This is a seperate project, not a part of the library
cmake_minimum_required(VERSION 3.28)

project(usage LANGUAGES CXX)

include(FetchContent)

FetchContent_Declare(
    mylibrary
    GIT_REPOSITORY https://github.com/vqk-2003/CMake-template.git
    GIT_TAG 16204725b0c7af5cb575fdce230a6300ff21101a
    SYSTEM
    GIT_SHALLOW
    FIND_PACKAGE_ARGS 1.0.0
)

FetchContent_MakeAvailable(mylibrary)

add_executable(${PROJECT_NAME} main.cpp)
target_link_libraries(${PROJECT_NAME} PRIVATE "mylibrary::mylibrary")