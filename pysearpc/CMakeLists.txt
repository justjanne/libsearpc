file(GLOB PYSEARPC_SOURCES *.py)
install(
    FILES ${PYSEARPC_SOURCES}
    DESTINATION ${Python3_SITELIB}/pysearpc
)
