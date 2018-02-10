
project(libtcod C)

file(GLOB TCOD_HEADERS include/*.h)
set(TCOD_SOURCES
    src/parser_c.c
    src/path_c.c
    src/sys_c.c
    #src/sys_opengl_c.c
    #src/sys_sdl_c.c
    #src/sys_sdl_img_bmp.c
    #src/sys_sdl_img_png.c
    src/sys_sdl2_c.c
    src/tree_c.c
    src/txtfield_c.c
    src/wrappers.c
    src/zip_c.c
    src/noise_c.c
    src/namegen_c.c
    src/mersenne_c.c
    src/list_c.c
    src/lex_c.c
    src/image_c.c
    src/heightmap_c.c
    src/fov_restrictive.c
    src/fov_recursive_shadowcasting.c
    src/fov_permissive2.c
    src/fov_diamond_raycasting.c
    src/fov_circular_raycasting.c
    src/fov_c.c
    src/console_rexpaint.c
    src/console_c.c
    src/color_c.c
    src/bsp_c.c
    src/bresenham_c.c
)

add_library(tcod STATIC ${TCOD_SOURCES} ${TCOD_HEADERS})
target_include_directories(tcod
    PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include
    PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src/zlib
)

if (MSVC)
    target_compile_definitions(tcod
        PUBLIC -D_CRT_SECURE_NO_WARNINGS
    )
endif()

target_compile_definitions(tcod
    PUBLIC -DLIBTCOD_EXPORTS -DNO_OPENGL
)

if (NOT MSVC)
    set_target_properties(tcod
        PROPERTIES
        CFLAGS -ansi
    )
endif()

if (EMSCRIPTEN)
    set(EMSETTINGS
        -s USE_WEBGL2=1 -s FULL_ES3=1
        -s OFFSCREENCANVAS_SUPPORT=1
        -s USE_SDL=2
        -s USE_ZLIB=1
        -s USE_PNG=1
        -s WASM=1
    )
    target_compile_definitions(tcod PUBLIC -DTCOD_EMSCRIPTEN -D_POSIX_SOURCE -D_GNU_SOURCE)
    set_target_properties(tcod
        PROPERTIES
        C_STANDARD 99
        CFLAGS "${EMSETTINGS}"
        LINK_FLAGS "-s WASM=1"
    )
endif()
