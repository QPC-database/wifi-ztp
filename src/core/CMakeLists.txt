
project(ztpcore)

add_library(ztpcore
    STATIC
    ""
)

option(CONFIG_EXPLICIT_AUTH_ON_CHIRP "Initiate DPP authentication on first chirp rx" OFF)

if (CONFIG_EXPLICIT_AUTH_ON_CHIRP)
    target_compile_definitions(ztpcore
        PRIVATE
            CONFIG_EXPLICIT_AUTH_ON_CHIRP=1
    )
endif(CONFIG_EXPLICIT_AUTH_ON_CHIRP)

add_subdirectory(biproviders)

target_sources(ztpcore
    PRIVATE
        ztp_configurator_config.c
        ztp_configurator.c
        ztp_enrollee_config.c
        ztp_enrollee.c
        ztp_settings.c
        ztp_wpa_supplicant.c
        ztp.c
)

target_include_directories(ztpcore
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}/biproviders
        ${CMAKE_CURRENT_SOURCE_DIR}/../dbus
        ${CMAKE_CURRENT_SOURCE_DIR}/../utils
        ${CMAKE_CURRENT_SOURCE_DIR}/../wpas
        ${CMAKE_CURRENT_SOURCE_DIR}/../wifi
)

target_link_libraries(ztpcore
    ${LIBSYSTEMD_TARGET}
    ${LIBJSONC_TARGET}
    azure_dps_service_client
    ztpdbus 
    ztputils
    ztpwpas
    ztpwifi
)
