cmake_minimum_required(VERSION 3.20)

project(KratosEngine
    DESCRIPTION "Game engine written in C/C++ with Python scripting."
    HOMEPAGE_URL "https://github.com/Soviet-Cat/KratosEngine"
    VERSION 1.0.0
    LANGUAGES C CXX
)

set(CMAKE_C_STANDARD 23)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

include(CMakePackageConfigHelpers)

file(GLOB KratosRuntimeSources "${CMAKE_CURRENT_SOURCE_DIR}/runtime/private/*.c" "${CMAKE_CURRENT_SOURCE_DIR}/runtime/private/*.cpp")
file(GLOB KratosRuntimeHeaders "${CMAKE_CURRENT_SOURCE_DIR}/runtime/public/*.h" "${CMAKE_CURRENT_SOURCE_DIR}/runtime/public/*.hpp")
add_library(KratosRuntime STATIC ${KratosRuntimeSources})
set_target_properties(KratosRuntime PROPERTIES
    VERSION ${PROJECT_VERSION}
    OUTPUT_NAME "KratosRuntime-${CMAKE_SYSTEM_NAME}-${CMAKE_SYSTEM_PROCESSOR}"
    PREFIX ""
)
target_include_directories(KratosRuntime 
    PRIVATE 
        "${CMAKE_CURRENT_SOURCE_DIR}/runtime/private" 
        "${CMAKE_CURRENT_SOURCE_DIR}/runtime/public" 
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/runtime/public>
        $<INSTALL_INTERFACE:include>
)
target_link_libraries(KratosRuntime PRIVATE)
target_compile_options(KratosRuntime PRIVATE -g3)
install(
    TARGETS KratosRuntime
    EXPORT KratosRuntimeTargets
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)
install(
    FILES ${KratosRuntimeHeaders}
    DESTINATION include/KratosEngine/
)
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/KratosRuntimeConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)
configure_package_config_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/RuntimeConfig.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/KratosRuntimeConfig.cmake"
    INSTALL_DESTINATION lib/cmake/KratosEngine/
)
install(
    FILES "${CMAKE_CURRENT_BINARY_DIR}/KratosRuntimeConfig.cmake" "${CMAKE_CURRENT_BINARY_DIR}/KratosRuntimeConfigVersion.cmake"
    DESTINATION lib/cmake/KratosEngine/
)
install(
    EXPORT KratosRuntimeTargets
    FILE KratosRuntimeTargets.cmake
    NAMESPACE KratosEngine::
    DESTINATION lib/cmake/KratosEngine/
)

file(GLOB KratosBuilderSources "${CMAKE_CURRENT_SOURCE_DIR}/builder/private/*.cpp" "${CMAKE_CURRENT_SOURCE_DIR}/builder/private/*.c")
file(GLOB KratosBuilderHeaders "${CMAKE_CURRENT_SOURCE_DIR}/builder/public/*.h" "${CMAKE_CURRENT_SOURCE_DIR}/builder/public/*.hpp")
add_executable(KratosBuilder ${KratosBuilderSources})
set_target_properties(KratosBuilder PROPERTIES
    VERSION ${PROJECT_VERSION}
    OUTPUT_NAME "KratosBuilder-${CMAKE_SYSTEM_NAME}-${CMAKE_SYSTEM_PROCESSOR}"
)
target_include_directories(KratosBuilder 
    PRIVATE 
        "${CMAKE_CURRENT_SOURCE_DIR}/builder/private" 
        "${CMAKE_CURRENT_SOURCE_DIR}/runtime/public" 
)
target_link_libraries(KratosBuilder PRIVATE KratosRuntime)
target_compile_options(KratosBuilder PRIVATE -g0)
install(
    TARGETS KratosBuilder
    EXPORT KratosBuilderTargets
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)
install(
    FILES ${KratosBuilderHeaders}
    DESTINATION include/KratosEngine/
)
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/KratosBuilderConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)
configure_package_config_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/BuilderConfig.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/KratosBuilderConfig.cmake"
    INSTALL_DESTINATION lib/cmake/KratosEngine/
)
install(
    FILES "${CMAKE_CURRENT_BINARY_DIR}/KratosBuilderConfig.cmake" "${CMAKE_CURRENT_BINARY_DIR}/KratosBuilderConfigVersion.cmake"
    DESTINATION lib/cmake/KratosEngine/
)
install(
    EXPORT KratosBuilderTargets
    FILE KratosBuilderTargets.cmake
    NAMESPACE KratosEngine::
    DESTINATION lib/cmake/KratosEngine/
)