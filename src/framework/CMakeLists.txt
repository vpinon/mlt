file(GLOB mlt_src *.c)
if(WIN32)
    list(APPEND mlt_src ../win32/win32.c ../win32/strptime.c)
endif()
add_library(mlt SHARED ${mlt_src})
set_target_properties (mlt PROPERTIES SOVERSION ${MLT_VERSION} VERSION 6)
if(WIN32)
    find_package(dlfcn-win32 REQUIRED)
    find_package(iconv REQUIRED)
    target_link_libraries(mlt dlfcn-win32::dl Iconv::Iconv)
    if(NODEPLOY)
        target_compile_definitions(mlt PRIVATE NODEPLOY)
    endif()
endif()

target_link_libraries(mlt Threads::Threads)
target_include_directories(mlt PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
target_include_directories(mlt PUBLIC  ${CMAKE_SOURCE_DIR}/src)

install(TARGETS mlt DESTINATION ${CMAKE_INSTALL_LIBDIR})

file(GLOB mlt_head *.h)
install(FILES ${mlt_head} DESTINATION include/mlt)

#pkg-config
#cmake
#configure_file(mlt.pc.in mlt.pc @ONLY)
#install (FILES
#    ${CMAKE_CURRENT_BINARY_DIR}/mlt.pc
#    DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig
#    COMPONENT Development)
#configure_file(MLTConfig.cmake.in MLTConfig.cmake @ONLY)
#configure_file(MLTConfigVersion.cmake.in MLTConfigVersion.cmake @ONLY)
#install (FILES
#    ${CMAKE_CURRENT_BINARY_DIR}/MLTConfig.cmake
#    ${CMAKE_CURRENT_BINARY_DIR}/MLTConfigVersion.cmake
#    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/mlt
#    COMPONENT Development)
