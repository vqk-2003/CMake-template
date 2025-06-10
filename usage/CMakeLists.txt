# This is a seperate project, not a part of the library
cmake_minimum_required(VERSION 3.28)

project(usage LANGUAGES CXX)

include(FetchContent)

# If find_package() failed i.e. cannot find mylibrary or its dependencies, mylibrary and its dependencies
# will be redownloaded even if mylibrary is already installed on your system.
FetchContent_Declare(
    mylibrary
    GIT_REPOSITORY https://github.com/vqk-2003/CMake-template.git
    GIT_TAG dec41cc5a8e7e9a1c32b026133dae56f2157670e
    SYSTEM
    GIT_SHALLOW
    FIND_PACKAGE_ARGS 1.0.0
)

FetchContent_MakeAvailable(mylibrary)

add_executable(${PROJECT_NAME} main.cpp)
target_link_libraries(${PROJECT_NAME} PRIVATE "mylibrary::mylibrary")