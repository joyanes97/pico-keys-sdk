set(PICO_KEYS_SDK_DIR ${CMAKE_CURRENT_LIST_DIR}/../../../..)

if(NOT CBOR_SOURCES)
    set(CBOR_SOURCES
        ${PICO_KEYS_SDK_DIR}/tinycbor/src/cborencoder.c
        ${PICO_KEYS_SDK_DIR}/tinycbor/src/cborparser.c
        ${PICO_KEYS_SDK_DIR}/tinycbor/src/cborparser_dup_string.c
    )
endif()

idf_component_register(
    SRCS ${CBOR_SOURCES}
    INCLUDE_DIRS ${PICO_KEYS_SDK_DIR}/tinycbor/src
)

idf_component_set_property(${COMPONENT_NAME} WHOLE_ARCHIVE ON)
