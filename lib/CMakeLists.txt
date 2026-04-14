set(libsearpc_SOURCES
    searpc-client.c
    searpc-server.c
    searpc-utils.c
    searpc-named-pipe-transport.c
)

set(libsearpc_HEADERS
    searpc-client.h
    searpc-server.h
    searpc-utils.h
    searpc.h
    searpc-named-pipe-transport.h
)

add_library(searpc SHARED ${libsearpc_SOURCES})
set_target_properties(searpc PROPERTIES
        VERSION 1.0.2
        SOVERSION 1
)

target_link_libraries(searpc
    PUBLIC
    ${GLIB_LIBRARIES}
    ${JANSSON_LIBRARIES}
    Threads::Threads
    ${LIB_WS32}
)

target_include_directories(searpc
    PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
)

install(TARGETS searpc
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
)

install(FILES ${libsearpc_HEADERS}
    DESTINATION include/searpc
)

install(PROGRAMS searpc-codegen.py
    DESTINATION bin
)
