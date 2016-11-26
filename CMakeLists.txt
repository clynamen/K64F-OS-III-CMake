cmake_minimum_required(VERSION 2.8)

project (k64f)

set(CMAKE_C_COMPILER "arm-none-eabi-gcc")
set(CMAKE_CXX_COMPILER "arm-none-eabi-g++")
set(CMAKE_COMMON_FLAGS "-mcpu=cortex-m4 -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16 -O0 -fmessage-length=0 -fsigned-char -ffunction-sections -fdata-sections  -g -DDEBUG -DCPU_MK64FN1M0VMD12 -DKDS -DFSL_RTOS_UCOSIII")
set(CMAKE_EXE_LINKER_FLAGS "-T '${CMAKE_SOURCE_DIR}/K64FN1Mxxx12_flash.ld' -Xlinker --gc-sections -Wl,-Map,'OS3-KSDK.map' -specs=nano.specs -specs=nosys.specs ")
set(CMAKE_SHARED_LIBRARY_LINK_C_FLAGS)
set(CMAKE_C_FLAGS ${CMAKE_COMMON_FLAGS})
set(CMAKE_ASM_FLAGS " -mcpu=cortex-m4 -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16 -O0 -fmessage-length=0 -fsigned-char -ffunction-sections -fdata-sections  -g -x assembler-with-cpp -DDEBUG -MMD -MP -DCPU_MK64FN1M0VMD12 ")
set(CMAKE_ASM_COMPILER "arm-none-eabi-gcc")
enable_language(ASM)

set(SRC_DIRS 
uCOS-III/Source 
uCOS-III/Ports/ARM-Cortex-M4/Generic/GNU 
uC-LIB 
uC-CPU 
uC-CPU/ARM-Cortex-M4/GNU 
FRDM-K64F/OS3-KSDK/
FRDM-K64F/BSP
FRDM-K64F/BSP/KSDK/
FRDM-K64F/BSP/KSDK/KDS/
KSDK/platform/system/interrupt/src 
KSDK/platform/system/clock 
KSDK/platform/CMSIS/Include/
KSDK/platform/CMSIS/Include/device
KSDK/platform/system/clock/MK64F12 
KSDK/platform/osa/
KSDK/rtos/uCOSIII/src/uCOS-III/Source/
KSDK/platform/startup/MK64F12 
KSDK/rtos/uCOSIII/src/uC-CPU/
KSDK/platform/osa/src 
KSDK/rtos/uCOSIII/src/uC-CPU/ARM-Cortex-M4/GNU
KSDK/platform/hal/uart 
KSDK/rtos/uCOSIII/src/uC-LIB/
KSDK/platform/hal/sim 
KSDK/rtos/uCOSIII/src/uCOS-III/Ports/ARM-Cortex-M4/Generic/GNU
KSDK/platform/hal/sim/MK64F12 
KSDK/platform/drivers/gpio/
KSDK/platform/hal/port 
KSDK/platform/drivers/uart
KSDK/platform/hal/mcg 
KSDK/platform/hal/gpio 
KSDK/platform/drivers/uart/src 
KSDK/platform/drivers/uart/common 
KSDK/platform/drivers/gpio/src 
KSDK/platform/drivers/gpio/common 
KSDK/platform/system/interrupt
KSDK/platform/utilities/
KSDK/boards/frdmk64f120m 
BSP 
BSP/KSDK 
BSP/KSDK/KDS 
APP)

set(CMAKE_ASM_COMPILER "arm-none-eabi-gcc")

set(SOURCES "")
foreach(SRC_DIR ${SRC_DIRS})
  file(GLOB DIR_SOURCES ${SRC_DIR}/*.c ${SRC_DIR}/*.S)
  set(SOURCES ${SOURCES} "${DIR_SOURCES}")
endforeach(SRC_DIR)

include_directories(
  ${SRC_DIRS}
)

add_executable(${PROJECT_NAME} ${SOURCES})
target_link_libraries(${PROJECT_NAME} m)

add_custom_command(TARGET ${PROJECT_NAME}
  COMMAND arm-none-eabi-objcopy -O binary ${PROJECT_NAME} ${PROJECT_NAME}.bin
  WORKING_DIRECTORY ${BUILD_DIRECTORY}
  COMMENT "objcopying ${PROJECT_NAME} to ${PROJECT_NAME}.bin")
add_custom_target(${PROJECT_NAME}.bin ALL DEPENDS ${PROJECT_NAME})
add_custom_target(flash 
  COMMAND JLinkExe -AutoConnect 1 -Device MK64FN1M0XXX12 -If JTAG -JTAGConf -1,-1 -Speed 4000 -CommanderScript ${CMAKE_SOURCE_DIR}/load.jlink
  WORKING_DIRECTORY ${BUILD_DIRECTORY}
  DEPENDS ${PROJECT_NAME})

