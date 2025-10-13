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

file(GLOB KratosRuntimeSources "${CMAKE_CURRENT_SOURCE_DIR}/runtime/private/*.cpp" "${CMAKE_CURRENT_SOURCE_DIR}/runtime/private/*.c")
add_library(KratosRuntime STATIC ${KratosRuntimeSources})
target_include_directories(KratosRuntime 
    PRIVATE 
        "${CMAKE_CURRENT_SOURCE_DIR}/runtime/private" 
    PUBLIC
        "${CMAKE_CURRENT_SOURCE_DIR}/runtime/public" 
)
target_link_libraries(KratosRuntime PRIVATE)
target_compile_options(KratosRuntime PRIVATE -g3)
set_target_properties(KratosRuntime PROPERTIES
    VERSION ${PROJECT_VERSION}
    OUTPUT_NAME "KratosRuntime-${CMAKE_SYSTEM_NAME}-${CMAKE_SYSTEM_PROCESSOR}"
    PREFIX ""
)

file(GLOB KratosBuilderSources "${CMAKE_CURRENT_SOURCE_DIR}/builder/private/*.cpp" "${CMAKE_CURRENT_SOURCE_DIR}/builder/private/*.c")
add_executable(KratosBuilder ${KratosBuilderSources})
target_include_directories(KratosBuilder 
    PRIVATE 
        "${CMAKE_CURRENT_SOURCE_DIR}/builder/private" 
        "${CMAKE_CURRENT_SOURCE_DIR}/runtime/public" 
    PUBLIC
)
target_link_libraries(KratosBuilder PRIVATE KratosRuntime)
target_compile_options(KratosBuilder PRIVATE -g0)
set_target_properties(KratosBuilder PROPERTIES
    VERSION ${PROJECT_VERSION}
    OUTPUT_NAME "KratosBuilder-${CMAKE_SYSTEM_NAME}-${CMAKE_SYSTEM_PROCESSOR}"
)