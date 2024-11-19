cmake_minimum_required(VERSION 3.13)

# Project name and language
project(ledblink LANGUAGES C)

# Set build type to Debug if not specified
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug)
endif()

# Option to enable hardware-specific build
option(USE_STM32 "Build for STM32 hardware" OFF)

if(USE_STM32)
    # Configure for STM32 hardware
    set(CMAKE_C_COMPILER arm-none-eabi-gcc)
    set(CMAKE_C_FLAGS "-mcpu=cortex-m4 -mthumb -O2 -g -Wall")
    set(CMAKE_EXE_LINKER_FLAGS "-T${CMAKE_SOURCE_DIR}/src/STM32F446RETX_FLASH.ld -Wl,--gc-sections")
    add_definitions(-DSTM32F446xx) # Define STM32-specific macros
    message(STATUS "Configured for STM32 hardware.")
else()
    # Configure for host (mocked build)
    set(CMAKE_C_FLAGS "-g -O0 -Wall")
    add_definitions(-DDEBUG) # Optional for host-side debug
    message(STATUS "Configured for host (mocked build).")
endif()

# Source files
set(SOURCES
    ../src/main.c  # Update the path if necessary
)

# Include directories (must be added after the target is created)
add_executable(ledblink ${SOURCES})
target_include_directories(ledblink PRIVATE ../include)

# Print a message indicating successful configuration
message(STATUS "Project configured for LED blink application.")

