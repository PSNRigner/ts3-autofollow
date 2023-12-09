cmake_minimum_required(VERSION 3.14.0)

project(auto_follow)

set(BUILD_SHARED_LIBS ON)

set(SOURCE_FILES src/plugin.c)

include_directories(include)

add_library(auto_follow ${SOURCE_FILES})

if (MSVC)
    target_compile_options(auto_follow PRIVATE)
else ()
    target_compile_options(auto_follow PRIVATE -Wall -fPIC)
endif ()

set_target_properties(
        auto_follow
        PROPERTIES
        OUTPUT_NAME "AutoFollow_win64"
        RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/plugins"
)

file(
        COPY_FILE
        "${CMAKE_CURRENT_SOURCE_DIR}/package.ini"
        "${CMAKE_CURRENT_BINARY_DIR}/package.ini"
)

add_custom_command(
        TARGET auto_follow
        COMMAND ${CMAKE_COMMAND} -E tar "cfv" AutoFollow.ts3_plugin --format=zip "${CMAKE_CURRENT_BINARY_DIR}/package.ini" "${CMAKE_CURRENT_BINARY_DIR}/plugins/AutoFollow_win64.dll"
)


