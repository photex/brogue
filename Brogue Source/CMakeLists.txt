
set(CMAKE_EXECUTABLE_SUFFIX ".html")

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
target_link_libraries(brogue tcod)

if (EMSCRIPTEN)
  set(EMSETTINGS
    -s USE_WEBGL2=1 -s FULL_ES3=1
    -s OFFSCREENCANVAS_SUPPORT=1
    -s USE_SDL=2
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
else()
  set_target_properties(brogue
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}
    C_STANDARD 99
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
    )
endif()
