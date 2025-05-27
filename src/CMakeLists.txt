# define GNU standard installation directories
include(GNUInstallDirs)

# Create target
# let the users specify whether they want a static or shared library
add_library(${PROJECT_NAME} mylibrary.cpp)

# this let cmake know we are linking to a target and generate better diagnoses
add_library("${PROJECT_NAME}::${PROJECT_NAME}" ALIAS ${PROJECT_NAME})

# set target properties
target_include_directories(${PROJECT_NAME}
    PUBLIC
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

# only add compiler flags when building with gcc or clang
target_compile_options(${PROJECT_NAME}
    PRIVATE
        $<$<COMPILE_LANG_AND_ID:CXX,Clang,GNU>:$<BUILD_INTERFACE:-Wall;-Wextra;-Wconversion;-Wsign-conversion;-pedantic-errors;-Werror>>
)

# installation instructions
# set installation location for the .cmake files
set(INSTALL_CONFIGDIR ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})

# copy the library target to the install location and associate the target to the export target
# LIBRARY is the destination for dynamic libraries, ARCHIVE is the destination for static libraries
install(TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}Targets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

# install the export target
install(EXPORT "${PROJECT_NAME}Targets"
    NAMESPACE ${PROJECT_NAME}::
    DESTINATION ${INSTALL_CONFIGDIR}
)

# copy header files to the install include directory
install(
    DIRECTORY ${PROJECT_SOURCE_DIR}/include/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

# helper function to create config files
include(CMakePackageConfigHelpers)

write_basic_package_version_file(
    ${PROJECT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)

configure_package_config_file(
    ${PROJECT_SOURCE_DIR}/cmake/${PROJECT_NAME}Config.cmake.in
    ${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    INSTALL_DESTINATION
        ${INSTALL_CONFIGDIR}
)

# install all the helper files
install(
    FILES
        ${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
        ${PROJECT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
    DESTINATION
        ${INSTALL_CONFIGDIR}
)