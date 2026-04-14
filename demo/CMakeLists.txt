add_custom_command(
    OUTPUT searpc-signature.h searpc-marshal.h
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_SOURCE_DIR}/lib/searpc-codegen.py ${CMAKE_CURRENT_SOURCE_DIR}/rpc_table.py ${CMAKE_CURRENT_BINARY_DIR}
    DEPENDS ${CMAKE_SOURCE_DIR}/lib/searpc-codegen.py ${CMAKE_CURRENT_SOURCE_DIR}/rpc_table.py
    COMMENT "Generating rpc header files for demo"
)

include_directories(${CMAKE_CURRENT_BINARY_DIR} ${CMAKE_SOURCE_DIR}/lib)

add_executable(searpc-demo-server
    test-object.c
    searpc-demo-server.c
    searpc-signature.h
    searpc-marshal.h
)
target_link_libraries(searpc-demo-server searpc)

add_executable(searpc-demo-client
    test-object.c
    searpc-demo-client.c
    searpc-signature.h
    searpc-marshal.h
)
target_link_libraries(searpc-demo-client searpc)

add_executable(searpc-async-client
    demo-async-client.c
    searpc-signature.h
    searpc-marshal.h
)
target_link_libraries(searpc-async-client searpc)
