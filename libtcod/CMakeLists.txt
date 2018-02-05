
project(libtcod CXX)

file(GLOB TCOD_FILES "src/*.c" "src/*.cpp")
add_library(tcod STATIC ${TCOD_FILES})
target_include_directories(tcod
  PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include
  PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src/zlib
  )

if (MSVC)
  target_compile_definitions(tcod
    PUBLIC -D_CRT_SECURE_NO_WARNINGS -DLIBTCOD_EXPORTS
    )
endif()

if (NOT MSVC)
  set_target_properties(tcod
    PROPERTIES
    CFLAGS -ansi
    )
endif()

if (EMSCRIPTEN)
  target_compile_definitions(tcod PUBLIC -DTCOD_EMSCRIPTEN -D_POSIX_SOURCE -D_GNU_SOURCE)
  set_target_properties(tcod
    PROPERTIES
    CFLAGS   -std=c++11 -s USE_WEBGL2=1 -s FULL_ES3=1 -s USE_ZLIB=1 -s USE_PNG=1 -s OFFSCREENCANVAS_SUPPORT=1
    CXXFLAGS -std=c++11 -s USE_WEBGL2=1 -s FULL_ES3=1 -s USE_ZLIB=1 -s USE_PNG=1 -s OFFSCREENCANVAS_SUPPORT=1
    )
endif()

target_link_libraries(tcod SDL2-static)

# file(GLOB TCODGUI_FILES "src/gui/*.cpp")
# add_library(tcod-gui STATIC ${TCODGUI_FILES})
# target_include_directories(tcod-gui PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include/gui)
# target_link_libraries(tcod-gui tcod)
