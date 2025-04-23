cmake_minimum_required(VERSION 3.20.0)
set(PLATFORM "ZEPHYR" CACHE STRING "Platform to target")
set(BOARD "rpi_4b")

if (NOT DEFINED LF_MAIN)
  set(LF_MAIN "PressureSensor")
endif()

if (NOT DEFINED LOG_LEVEL)
  set(LOG_LEVEL "LF_LOG_LEVEL_WARN")
endif()

set(ZEPHYR_EXTRA_MODULES "${CMAKE_SOURCE_DIR}/modules/HX711")
find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})
find_package(Zephyr REQUIRED HINTS ${CMAKE_CURRENT_SOURCE_DIR}/deps/zephyr)
project(reactor-uc-zephyr)

set(LF_MAIN_TARGET app)

include($ENV{REACTOR_UC_PATH}/cmake/lfc.cmake)

lf_setup()
lf_run_lfc(${CMAKE_CURRENT_SOURCE_DIR}/src ${LF_MAIN})
lf_build_generated_code(${LF_MAIN_TARGET} ${CMAKE_CURRENT_SOURCE_DIR}/src-gen/${LF_MAIN})
