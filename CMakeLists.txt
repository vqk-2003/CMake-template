# the minimum version I think modern projects should set
cmake_minimum_required(VERSION 3.15)
project(mylibrary VERSION 0.0.1 LANGUAGES CXX)

# use c++11 for now
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_subdirectory(src)
add_subdirectory(test)