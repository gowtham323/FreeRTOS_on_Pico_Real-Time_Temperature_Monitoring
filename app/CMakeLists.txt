cmake_minimum_required(VERSION 3.13)

# Pull in SDK (must be before project)
include($ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake)

# Pull in FreeRTOS
include($ENV{FREERTOS_KERNEL_PATH}/portable/ThirdParty/GCC/RP2040/FreeRTOS_Kernel_import.cmake)

project(app C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Enable USB CDC for serial communication
add_compile_definitions(PICO_STDIO_USB)

# Initialize the SDK
pico_sdk_init()

# Add the executable
add_executable(app main.c)

# Specify any necessary include directories
target_include_directories(app PRIVATE 
    ${CMAKE_CURRENT_LIST_DIR}
    ${PICO_SDK_PATH}/src/rp2_common/hardware_adc/include #for ADC hardware include    
)

# Pull in common dependencies
target_link_libraries(app pico_stdlib FreeRTOS-Kernel FreeRTOS-Kernel-Heap4 hardware_adc)

# Create map/bin/hex/uf2 file etc.
pico_add_extra_outputs(app)

# Enable USB output, disable UART output
pico_enable_stdio_usb(app 1)
pico_enable_stdio_uart(app 0)  # Change this to 0 if you want to disable UART output
