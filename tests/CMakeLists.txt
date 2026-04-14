add_custom_command(
    OUTPUT searpc-signature.h searpc-marshal.h
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_SOURCE_DIR}/lib/searpc-codegen.py ${CMAKE_CURRENT_SOURCE_DIR}/rpc_table.py ${CMAKE_CURRENT_BINARY_DIR}
    DEPENDS ${CMAKE_SOURCE_DIR}/lib/searpc-codegen.py ${CMAKE_CURRENT_SOURCE_DIR}/rpc_table.py
    COMMENT "Generating rpc header files for tests"
)

add_custom_command(
    OUTPUT clar.suite
    COMMAND ${Python3_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/generate.py ${CMAKE_CURRENT_SOURCE_DIR}
    COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/clar.suite ${CMAKE_CURRENT_BINARY_DIR}/clar.suite
    COMMAND ${CMAKE_COMMAND} -E remove ${CMAKE_CURRENT_SOURCE_DIR}/clar.suite
    DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/generate.py ${CMAKE_CURRENT_SOURCE_DIR}/searpc.c
    COMMENT "Generating clar suite"
)

include_directories(${CMAKE_CURRENT_BINARY_DIR} ${CMAKE_SOURCE_DIR}/lib)

add_executable(test-searpc
    searpc.c
    clar.c
    main.c
    searpc-signature.h
    searpc-marshal.h
    clar.suite
)

target_link_libraries(test-searpc searpc)

add_test(NAME searpc-test COMMAND test-searpc)
