project(MLT)
set(MLT_VERSION 6.17.0)
cmake_minimum_required(VERSION 3.0)
find_package(PkgConfig REQUIRED)
include(GNUInstallDirs)
option(GPL "Enable GPLv2 modules" ON)
option(GPL3 "Enable GPLv3 modules" ON)
if(WIN32)
  option(NODEPLOY "Keep bin/ lib/ layout on Windows" ON)
endif()
find_package(Threads REQUIRED)
add_subdirectory(src/framework)
add_subdirectory(src/mlt++)
#add_subdirectory(src/swig)
#file(GLOB modules src/modules/*/)
set(modules
    src/modules/avformat
    src/modules/core
    src/modules/decklink
    src/modules/frei0r
    src/modules/feeds
    src/modules/gtk2
    src/modules/kdenlive
    src/modules/plus
    src/modules/plusgpl
    src/modules/qt
    src/modules/sdl2
    src/modules/xml
)
foreach(module ${modules})
    add_subdirectory(${module})
endforeach()
