
add_executable(brogue
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
target_compile_definitions(brogue PRIVATE -DBROGUE_TCOD)
target_include_directories(brogue
  PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/BrogueCode
  PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/PlatformCode
  )
target_link_libraries(brogue SDL2main tcod)

if (EMSCRIPTEN)
  target_compile_definitions(brogue PUBLIC -DTCOD_EMSCRIPTEN -D_POSIX_SOURCE -D_GNU_SOURCE)
  set_target_properties(brogue
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
    CFLAGS   -s USE_WEBGL2=1 -s FULL_ES3=1 -s USE_ZLIB=1 -s USE_PNG=1 -s OFFSCREENCANVAS_SUPPORT=1
    CXXFLAGS -s USE_WEBGL2=1 -s FULL_ES3=1 -s USE_ZLIB=1 -s USE_PNG=1 -s OFFSCREENCANVAS_SUPPORT=1
    LINK_FLAGS "-s WASM=1"
    )
else()
  target_compile_definitions(brogue PUBLIC -DTCOD_WINDOWS)
  set_target_properties(brogue
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}
    C_STANDARD C99
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
    )
endif()