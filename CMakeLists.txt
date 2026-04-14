cmake_minimum_required(VERSION 3.10)
project(libsearpc VERSION 3.1.0 LANGUAGES C)

option(BUILD_DEMOS "Build demo programs" ON)
option(BUILD_TESTS "Build tests" ON)

find_package(PkgConfig REQUIRED)
find_package(Python3 REQUIRED COMPONENTS Interpreter)
find_package(Threads REQUIRED)

pkg_check_modules(GLIB REQUIRED gobject-2.0 gio-2.0)
include_directories(${GLIB_INCLUDE_DIRS} ${JANSSON_INCLUDE_DIRS})
link_directories(${GLIB_LIBRARY_DIRS} ${JANSSON_LIBRARY_DIRS})
add_definitions(${GLIB_CFLAGS_OTHER} ${JANSSON_CFLAGS_OTHER})

pkg_check_modules(JANSSON REQUIRED jansson>=2.2.1)

if(WIN32)
    add_definitions(-D_WIN32_WINNT=0x0600)
    set(LIB_WS32 ws2_32)
endif()

add_subdirectory(lib)
add_subdirectory(pysearpc)

if(BUILD_DEMOS)
    add_subdirectory(demo)
endif()

if(BUILD_TESTS)
    enable_testing()
    add_subdirectory(tests)
endif()

set(prefix ${CMAKE_INSTALL_PREFIX})
set(exec_prefix ${CMAKE_INSTALL_PREFIX})
set(libdir ${CMAKE_INSTALL_PREFIX}/lib)
set(includedir ${CMAKE_INSTALL_PREFIX}/include)
set(VERSION ${PROJECT_VERSION})

configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/libsearpc.tpl.pc
    ${CMAKE_CURRENT_BINARY_DIR}/libsearpc.pc
    @ONLY
)
install(
    FILES ${CMAKE_CURRENT_BINARY_DIR}/libsearpc.pc
    DESTINATION lib/pkgconfig
)
