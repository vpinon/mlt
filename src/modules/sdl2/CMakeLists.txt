find_package(SDL2 QUIET)
if(TARGET SDL2::SDL2)
    add_library(sdl2 ALIAS SDL2::SDL2)
else()
    pkg_check_modules(sdl2 IMPORTED_TARGET GLOBAL sdl2)
    if(TARGET PkgConfig::sdl2)
        add_library(sdl2 ALIAS PkgConfig::sdl2)
    endif()
endif()
if(TARGET sdl2)
    file(GLOB mltsdl2_src *.c)
    add_library(mltsdl2 MODULE ${mltsdl2_src})
    target_link_libraries(mltsdl2 mlt m Threads::Threads sdl2)
    install(TARGETS mltsdl2 LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}/mlt)
    file(GLOB yml *.yml)
    install(FILES ${yml} DESTINATION ${CMAKE_INSTALL_DATADIR}/mlt/sdl2)
endif()
