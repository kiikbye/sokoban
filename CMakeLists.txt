cmake_minimum_required(VERSION 3.20)
project(Sokoban LANGUAGES CXX)

find_package(SDL2 CONFIG REQUIRED)
find_package(OpenGL REQUIRED)

add_executable(sokoban WIN32 sokoban_gl.cpp)
target_compile_features(sokoban PRIVATE cxx_std_17)

if(TARGET SDL2::SDL2main)
    target_link_libraries(sokoban PRIVATE SDL2::SDL2main)
endif()
target_link_libraries(sokoban PRIVATE SDL2::SDL2 OpenGL::GL)

get_target_property(sdl2_library_type SDL2::SDL2 TYPE)
if(WIN32 AND sdl2_library_type STREQUAL "SHARED_LIBRARY")
    add_custom_command(TARGET sokoban POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
            $<TARGET_FILE:SDL2::SDL2> $<TARGET_FILE_DIR:sokoban>
        VERBATIM)
endif()
