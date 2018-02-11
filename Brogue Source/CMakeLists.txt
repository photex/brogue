
set(CMAKE_EXECUTABLE_SUFFIX ".html")

file(GLOB TCOD_HEADERS ${CMAKE_SOURCE_DIR}/libtcod/include/*.h)
set(TCOD_SOURCES
  ${CMAKE_SOURCE_DIR}/libtcod/src/parser_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/path_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/sys_c.c
  #${CMAKE_SOURCE_DIR}/libtcod/src/sys_opengl_c.c
  #${CMAKE_SOURCE_DIR}/libtcod/src/sys_sdl_c.c
  #${CMAKE_SOURCE_DIR}/libtcod/src/sys_sdl_img_bmp.c
  #${CMAKE_SOURCE_DIR}/libtcod/src/sys_sdl_img_png.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/sys_sdl2_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/tree_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/txtfield_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/wrappers.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/zip_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/noise_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/namegen_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/mersenne_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/list_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/lex_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/image_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/heightmap_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/fov_restrictive.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/fov_recursive_shadowcasting.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/fov_permissive2.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/fov_diamond_raycasting.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/fov_circular_raycasting.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/fov_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/console_rexpaint.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/console_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/color_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/bsp_c.c
  ${CMAKE_SOURCE_DIR}/libtcod/src/bresenham_c.c
)

set(BROGUE_SOURCES
  BrogueCode/Architect.c
  BrogueCode/Combat.c
	BrogueCode/Dijkstra.c
	BrogueCode/Globals.c
	BrogueCode/IO.c
	BrogueCode/Buttons.c
	BrogueCode/MainMenu.c
	BrogueCode/Items.c
	BrogueCode/Light.c
	BrogueCode/Monsters.c
	BrogueCode/Movement.c
	BrogueCode/RogueMain.c
	BrogueCode/Random.c
	BrogueCode/Recordings.c
	PlatformCode/main.c
	PlatformCode/platformdependent.c
	PlatformCode/tcod-platform.c
)

add_executable(brogue ${TCOD_HEADERS} ${TCOD_SOURCES} ${BROGUE_SOURCES})
target_compile_definitions(brogue
  PUBLIC -DBROGUE_TCOD -DLIBTCOD_EXPORTS -DNO_OPENGL -DTCOD_EMSCRIPTEN -D_POSIX_SOURCE -D_GNU_SOURCE
)
target_include_directories(brogue
  PRIVATE ${CMAKE_SOURCE_DIR}/libtcod/include
  PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/BrogueCode
  PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/PlatformCode
)
target_link_libraries(brogue SDL2-static SDL2main)

if (EMSCRIPTEN)
  set(EMSETTINGS
    -s USE_WEBGL2=1 -s FULL_ES3=1
    -s OFFSCREENCANVAS_SUPPORT=1
    -s USE_ZLIB=1
    -s USE_PNG=1
    -s WASM=1
  )
  set_target_properties(brogue
    PROPERTIES
    C_STANDARD 99
    CFLAGS "${EMSETTINGS}"
    LINK_FLAGS "-s WASM=1 --preload-file ${CMAKE_SOURCE_DIR}/fonts@/fonts"
    )
endif()
