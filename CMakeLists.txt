cmake_minimum_required(VERSION 3.9)

project(brogue C CXX)

set(CMAKE_CXX_STANDARD 11)

#if (NOT EMSCRIPTEN)
#  set(SDL_SHARED_ENABLED_BY_DEFAULT OFF)
#  add_subdirectory(SDL2-2.0.7)
#endif()

add_subdirectory(libtcod)

add_subdirectory("Brogue Source")
