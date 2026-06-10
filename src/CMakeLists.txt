add_library(Kelp-SD-SDIO-SPI INTERFACE)

pico_generate_pio_header(Kelp-SD-SDIO-SPI ${CMAKE_CURRENT_LIST_DIR}/sd_driver/SDIO/rp2040_sdio.pio)

target_compile_definitions(Kelp-SD-SDIO-SPI INTERFACE
    PICO_MAX_SHARED_IRQ_HANDLERS=8u
)
# target_compile_options(Kelp-SD-SDIO-SPI INTERFACE -ffile-prefix-map=${CMAKE_CURRENT_LIST_DIR}=)

target_sources(Kelp-SD-SDIO-SPI INTERFACE
    ${CMAKE_CURRENT_LIST_DIR}/sd_driver/dma_interrupts.c
    ${CMAKE_CURRENT_LIST_DIR}/sd_driver/sd_card.c
    ${CMAKE_CURRENT_LIST_DIR}/sd_driver/sd_timeouts.c
    ${CMAKE_CURRENT_LIST_DIR}/sd_driver/SDIO/rp2040_sdio.c
    ${CMAKE_CURRENT_LIST_DIR}/sd_driver/SDIO/sd_card_sdio.c
    ${CMAKE_CURRENT_LIST_DIR}/sd_driver/SPI/my_spi.c
    ${CMAKE_CURRENT_LIST_DIR}/sd_driver/SPI/sd_card_spi.c
    ${CMAKE_CURRENT_LIST_DIR}/sd_driver/SPI/sd_spi.c
    ${CMAKE_CURRENT_LIST_DIR}/src/crc.c
    ${CMAKE_CURRENT_LIST_DIR}/src/my_debug.c
    ${CMAKE_CURRENT_LIST_DIR}/src/util.c
)
target_include_directories(Kelp-SD-SDIO-SPI INTERFACE
    sd_driver
    include
)

if(PICO_RISCV)
    set(HWDEP_LIBS hardware_watchdog)
else()
    set(HWDEP_LIBS cmsis_core)
endif()

target_link_libraries(Kelp-SD-SDIO-SPI INTERFACE
    hardware_dma
    hardware_pio
    hardware_spi
    hardware_sync
    pico_aon_timer
    pico_stdlib
    ${HWDEP_LIBS}
)
