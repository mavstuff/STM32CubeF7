/**
  @page HID_Standalone USB Host Human Interface (HID) application
  
  @verbatim
  ******************************************************************************
  * @file    USB_Host/HID_Standalone/readme.txt 
  * @author  MCD Application Team
  * @brief   Description of the USB Host HID application.
  ******************************************************************************
  *
  * Copyright (c) 2016 STMicroelectronics. All rights reserved.
  *
  * This software component is licensed by ST under BSD 3-Clause license,
  * the "License"; You may not use this file except in compliance with the
  * License. You may obtain a copy of the License at:
  *                       opensource.org/licenses/BSD-3-Clause
  *
  ******************************************************************************
  @endverbatim

@par Application Description 

Use of the USB host application based on the HID class.

This is a typical application on how to use the STM32F723xx USB OTG Host peripheral to interact with an USB 
HID Device such as a Mouse or a Keyboard.

At the beginning of the main program the HAL_Init() function is called to reset all the peripherals,
initialize the Flash interface and the systick. The user is provided with the SystemClock_Config()
function to configure the system clock (SYSCLK) to run at 216 MHz. The Full Speed (FS) USB module uses
internally a 48-MHz clock which is coming from the main PLL.

When the application is started, the connected HID device (Mouse/Keyboard) is detected in HID mode and 
gets initialized. The STM32 MCU behaves as a HID Host, it enumerates the device and extracts VID, PID, 
manufacturer name, Serial no and product name information and displays it on the LCD screen. 
This application is based on interacting with a HID device (Mouse/Keyboard) trough a HID routine.

A menu is displayed and the user can select any operation from the menu using the Touch Screen:
 - "Start HID" operation starts the appropriate HID application to the attached HID Device.
   - "Start Mouse / Re-Initialize" operation starts Mouse HID application. Moving the mouse will move
    the pointer in the display rectangle and if a button is pressed, the corresponding rectangle will be
    highlighted in Blue.
   - "Start Keyboard / Clear" operation starts Keyboard HID application. Taped Keyboard characters are 
   displayed on the LCD screen.
 - "Re-Enumerate" operation performs a new Enumeration of the device.
 
@note In case of using an AZERTY keyboard, user should add "AZERTY_KEYBOARD" define to ensure correct 
      displaying taped characters.

@note Care must be taken when using HAL_Delay(), this function provides accurate delay (in milliseconds)
      based on variable incremented in SysTick ISR. This implies that if HAL_Delay() is called from
      a peripheral ISR process, then the SysTick interrupt must have higher priority (numerically lower)
      than the peripheral interrupt. Otherwise the caller ISR process will be blocked.
      To change the SysTick interrupt priority you have to use HAL_NVIC_SetPriority() function.
      
@note The application needs to ensure that the SysTick time base is always set to 1 millisecond
      to have correct HAL operation.

For more details about the STM32Cube USB Host library, please refer to UM1720  
"STM32Cube USB Host library".


@par USB Library Configuration

It is possible to fine tune needed USB Host features by modifying defines values in USBH configuration
file �usbh_conf.h� available under the project includes directory, in a way to fit the application
requirements, such as:
- Level of debug: USBH_DEBUG_LEVEL
                  0: No debug messages
                  1: Only User messages are shown
                  2: User and Error messages are shown
                  3: All messages and internal debug messages are shown
   By default debug messages are displayed on the debugger IO terminal; to redirect the Library
   messages on the LCD screen, lcd_log.c driver need to be added to the application sources.


@par Keywords

Connectivity, USB_Host, USB, HID, Full Speed, High Speed, Arduino, TFT, Shield, Joystick, Mouse, Keyboard, enumerate

@Note�If the user code size exceeds the DTCM-RAM size or starts from internal cacheable memories (SRAM1 and SRAM2),that is shared between several processors,
 �����then it is highly recommended to enable the CPU cache and maintain its coherence at application level.
������The address and the size of cacheable buffers (shared between CPU and other masters)  must be properly updated to be aligned to cache line size (32 bytes).

@Note It is recommended to enable the cache and maintain its coherence, but depending on the use case
����� It is also possible to configure the MPU as "Write through", to guarantee the write access coherence.
������In that case, the MPU must be configured as Cacheable/Bufferable/Not Shareable.
������Even though the user must manage the cache coherence for read accesses.
������Please refer to the AN4838 �Managing memory protection unit (MPU) in STM32 MCUs�
������Please refer to the AN4839 �Level 1 cache on STM32F7 Series�

@par Directory contents

  - USB_Host/HID_Standalone/Src/main.c                  Main program
  - USB_Host/HID_Standalone/Src/system_stm32f7xx.c      STM32F7xx system clock configuration file
  - USB_Host/HID_Standalone/Src/stm32f7xx_it.c          Interrupt handlers
  - USB_Host/HID_Standalone/Src/menu.c                  HID State Machine
  - USB_Host/HID_Standalone/Src/usbh_conf.c             General low level driver configuration
  - USB_Host/HID_Standalone/Src/mouse.c                 HID mouse functions file
  - USB_Host/HID_Standalone/Src/keybaord.c              HID keyboard functions file
  - USB_Host/HID_Standalone/Inc/main.h                  Main program header file
  - USB_Host/HID_Standalone/Inc/stm32f7xx_it.h          Interrupt handlers header file
  - USB_Host/HID_Standalone/Inc/lcd_log_conf.h          LCD log configuration file
  - USB_Host/HID_Standalone/Inc/stm32f7xx_hal_conf.h    HAL configuration file
  - USB_Host/HID_Standalone/Inc/usbh_conf.h             USB Host driver Configuration file
 

@par Hardware and Software environment

  - This application runs on STM32F723xx devices.
    
  - This application has been tested with STMicroelectronics STM32F723E-Discovery Rev.C 
    boards and can be easily tailored to any other supported device 
    and development board.

  - STM32F723E-Discovery Set-up
    - Plug a USB device mouse or keyboard into the STM32F723E-Discovery board 
	through 'USB micro A-Male to A-Female' cable


@par How to use it ?

In order to make the program work, you must do the following :
 - Open your preferred toolchain 
 - Rebuild all files and load your image into target memory
 - In the workspace toolbar select the project configuration:
   - STM32F723E_DISCOVERY_USBH-HS: to configure the project for STM32F723xx devices using USB OTG HS peripheral
   - STM32F723E_DISCOVERY_USBH-FS: to configure the project for STM32F723xx devices using USB OTG FS peripheral
 - Run the application
 
 * <h3><center>&copy; COPYRIGHT STMicroelectronics</center></h3>
 */
