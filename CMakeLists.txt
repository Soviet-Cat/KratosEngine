cmake_minimum_required(VERSION 3.20)

project(KratosEngine)

set(SDL_SHARED OFF CACHE BOOL "Build SDL statically.")
set(SDL_STATIC ON CACHE BOOL "Build SDL statically.")
add_subdirectory("${CMAKE_CURRENT_SOURCE_DIR}/vendor/SDL3")

add_library(KratosEngine STATIC)

set_target_properties(KratosEngine PROPERTIES
    OUTPUT_NAME "${PROJECT_NAME}"
    VERSION "${PROJECT_VERSION}"
)

target_include_directories(KratosEngine
    PRIVATE 
        "${CMAKE_CURRENT_SOURCE_DIR}/include"
        "${CMAKE_CURRENT_SOURCE_DIR}/vendor/SDL3/include"
    PUBLIC 
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include> 
        $<INSTALL_INTERFACE:include>
)

file(GLOB_RECURSE KratosEngine_SOURCES 
    "src/*.cpp" 
    "src/*.c" 
    "src/**/*.cpp" 
    "src/**/*.c"
)
target_sources(KratosEngine PRIVATE ${KratosEngine_SOURCES})

target_link_libraries(KratosEngine PRIVATE SDL3-static)
target_compile_definitions(KratosEngine PRIVATE SDL_STATIC)

install(DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/include/" DESTINATION "include/KratosEngine")
install(TARGETS KratosEngine 
        RUNTIME DESTINATION "bin" 
        LIBRARY DESTINATION "lib" 
        ARCHIVE DESTINATION "lib"
)

include(CMakePackageConfigHelpers)
configure_package_config_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/Config.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/Config.cmake"
    INSTALL_DESTINATION "lib/cmake/KratosEngine"
)
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/Config.cmake"
        DESTINATION "lib/cmake/KratosEngine"
)