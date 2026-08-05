set(PICO_KEYS_SDK_DIR ${CMAKE_CURRENT_LIST_DIR}/../../../..)

if(PICO_FIDO_EMBEDDED AND NOT PICO_KEYS_SOURCES)
    set(USB_ITF_HID 1)
    set(USB_ITF_CCID 0)
    set(ENABLE_OATH_APP 0)
    set(ENABLE_OTP_APP 0)
    set(ENABLE_PQC 0)
    add_compile_definitions(PICO_FIDO_EMBEDDED=1 USB_ITF_HID=1)
    include(${PICO_KEYS_SDK_DIR}/pico_keys_sdk_import.cmake)
endif()

set(PICO_KEYS_INCLUDE_DIRS
    ${PICO_KEYS_SDK_DIR}/src
    ${PICO_KEYS_SDK_DIR}/src/fs
    ${PICO_KEYS_SDK_DIR}/src/rng
    ${PICO_KEYS_SDK_DIR}/src/usb
    ${PICO_KEYS_SDK_DIR}/src/led
    ${PICO_KEYS_SDK_DIR}/tinycbor/src
)

set(PICO_KEYS_REQUIRES
    bootloader_support
    esp_partition
    efuse
    mbedtls
    tinycbor
)

if(NOT PICO_FIDO_EMBEDDED)
    list(APPEND PICO_KEYS_REQUIRES esp_tinyusb)
endif()

if(ENABLE_PQC)
    list(APPEND PICO_KEYS_INCLUDE_DIRS
        ${PICO_KEYS_SDK_DIR}/mlkem/mlkem
        ${PICO_KEYS_SDK_DIR}/config/mlkem
    )
    list(APPEND PICO_KEYS_REQUIRES
        mlkem512
        mlkem768
        mlkem1024
    )
endif()

idf_component_register(
    SRCS ${PICO_KEYS_SOURCES}
    INCLUDE_DIRS ${PICO_KEYS_INCLUDE_DIRS}
    REQUIRES ${PICO_KEYS_REQUIRES}
)

idf_component_set_property(${COMPONENT_NAME} WHOLE_ARCHIVE ON)
