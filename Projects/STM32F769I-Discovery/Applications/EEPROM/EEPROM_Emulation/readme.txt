/**
  @page EEPROM_Emulation application to show an eeprom emulation

  @verbatim
  ******************************************************************************
  * @file    EEPROM_Emulation/readme.txt 
  * @author  MCD Application Team
  * @brief   Description of the EEPROM_Emulation application.
  ******************************************************************************
  *
  * Copyright (c) 2017 STMicroelectronics International N.V. All rights reserved.
  *
  * Redistribution and use in source and binary forms, with or without 
  * modification, are permitted, provided that the following conditions are met:
  *
  * 1. Redistribution of source code must retain the above copyright notice, 
  *    this list of conditions and the following disclaimer.
  * 2. Redistributions in binary form must reproduce the above copyright notice,
  *    this list of conditions and the following disclaimer in the documentation
  *    and/or other materials provided with the distribution.
  * 3. Neither the name of STMicroelectronics nor the names of other 
  *    contributors to this software may be used to endorse or promote products 
  *    derived from this software without specific written permission.
  * 4. This software, including modifications and/or derivative works of this 
  *    software, must execute solely and exclusively on microcontroller or
  *    microprocessor devices manufactured by or for STMicroelectronics.
  * 5. Redistribution and use of this software other than as permitted under 
  *    this license is void and will automatically terminate your rights under 
  *    this license. 
  *
  * THIS SOFTWARE IS PROVIDED BY STMICROELECTRONICS AND CONTRIBUTORS "AS IS" 
  * AND ANY EXPRESS, IMPLIED OR STATUTORY WARRANTIES, INCLUDING, BUT NOT 
  * LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY, FITNESS FOR A 
  * PARTICULAR PURPOSE AND NON-INFRINGEMENT OF THIRD PARTY INTELLECTUAL PROPERTY
  * RIGHTS ARE DISCLAIMED TO THE FULLEST EXTENT PERMITTED BY LAW. IN NO EVENT 
  * SHALL STMICROELECTRONICS OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
  * INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
  * LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, 
  * OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF 
  * LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING 
  * NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE,
  * EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
  *
  ******************************************************************************
  @endverbatim

@par Application Description 

This application describes the software solution for substituting standalone EEPROM
by emulating the EEPROM mechanism using the on-chip Flash of STM32F77x devices.

Please refer to AN3969 for further details regarding this application.

The application requires two Flash memory sectors of identical size allocated to non-volatile data:
one that is initially erased, and offers byte-by-byte programmability; the other that is ready
to take over when the former sector needs to be garbage-collected. A header field that occupies
the first half word (16-bit) of each sector indicates the sector status. Each of these sectors is
considered as a page.
The sectors used on Flash Single and dual Bank are as follows:
  -Dual bank flash: 16KB (Sector12, Sector13)
  -Single bank flash: 32KB (Sector2, Sector3)

If the flash configuration is Dual bank mode uncomment the #define USE_DUAL_BANK_FLASH  in main.h to emulate
an EPROOM with on a dual bank flash, otherwise the default flash configuration is a Single bank flash.

@note You have to initially configure the FLASH memory at Single or Dual Bank mode using STM32 ST-LINK 
      Utilities or any similar tool (check "nDBANK" box) according to the flash mode selected on main.h.

@note Using the Dual bank mode, the firmware code is executed from Sector0 on Bank1 and the Read/Write
      operations on EEPROM are done on Bank2.
	  
STM32F769I-Discovery's LEDs can be used to monitor the application status:
  - LED2 is ON when the example runs successfully.
  - LED1 is toggled with a Delay of 1 second in case of Flash configuration error, otherwise The LED1 is ON
    in case of case of EPPROM initialization, write or read error.

@note Care must be taken when using HAL_Delay(), this function provides accurate delay (in milliseconds)
      based on variable incremented in SysTick ISR. This implies that if HAL_Delay() is called from
      a peripheral ISR process, then the SysTick interrupt must have higher priority (numerically lower)
      than the peripheral interrupt. Otherwise the caller ISR process will be blocked.
      To change the SysTick interrupt priority you have to use HAL_NVIC_SetPriority() function.
      
@note The application need to ensure that the SysTick time base is always set to 1 millisecond
      to have correct HAL operation.

@par Keywords

EEPROM, Emulation, Flash, Program, Erase, Sectors, OPCODE

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

 - EEPROM_Emulation/Inc/stm32f7xx_hal_conf.h          HAL Configuration file
 - EEPROM_Emulation/Inc/main.h                        Header for main.c module
 - EEPROM_Emulation/Inc/eeprom.h                      Header for eeprom.c module 
 - EEPROM_Emulation/Inc/stm32f7xx_it.h                Header for stm32f7xx_it.c
 - EEPROM_Emulation/Src/main.c                        Main program
 - EEPROM_Emulation/Src/eeprom.c                      EEPROM program
 - EEPROM_Emulation/Src/stm32f7xx_it.c                Interrupt handlers
 - EEPROM_Emulation/Src/system_stm32f7xx.c            STM32F77x system clock configuration file 
    

@par Hardware and Software environment 

  - This application runs on STM32F767xx/STM32F769xx/STM32F777xx/STM32F779xx devices.
    
  - This application has been tested with STM32F769I-Discovery board and can be
    easily tailored to any other supported device and development board.

Table 1. EEPROM application implementation on STM32F769I-Discovery
+------------+--------------------------------------------------------------+-----------------------+
|  Platform  |                  Implementation                              |  Configuration        |
+------------+--------------------------------------------------------------+-----------------------+
|  Firmware  | The EEPROM program is located at 0x08000000. The Flash       |                       |
|            | routines (program/erase) are executed from the Flash memory. |                       |
|            | The size of this program is about 6 Kbytes and programmed on:|  Sector0              |
|            | -------------------------------------------------------------|-----------------------|
|            | The flash zone used for the EEPROM emulation is located at:  |                       |
|            |   -Dual bank flash:   0x08100000                             | 32 MB (Sector12 + 13) |
|            |   -Single bank flash: 0x08010000                             | 64 KB (Sector2 + 3)   |
+------------+--------------------------------------------------------------+-----------------------+

@note Before using the application, ensure that dedicated sectors for EEPROM (Sector2 and Sector3) on Single Bank
      or sectors (Sector12 and Sector13) on Dual Bank mode are erased to avoid a match with a valid page OPCODE.

Following picture illustrates the situation in program memory:

                                Single bank flash            Dual bank flash
  Flash End       0x08200000  /----------------------\    /----------------------\  0x08200000 Flash End
                              |                      |    |                      |
                              |  Sector4 -> Sector11 |    | Sector14 -> Sector23 |
                              |                      |    |                      |
  Sector4 (128KB) 0x08018000  |----------------------|    |----------------------|  0x08108000 Sector14 (64KB)
                              |   Flash used for     |    |  Flash used for      |
                              |   EEPROM emulation   |    |  EEPROM emulation    |
  Sector3 (32KB)  0x08018000  |   mechanism          |    |  mechanism           |  0x08104000 Sector13 (16KB)
                              |                      |    |                      |
                              |                      |    |                      |
  Sector2 (32KB)  0x08010000  |----------------------|    |----------------------|  0x08100000 Sector12 (16KB)
              :               |                      |    |                      |           :
              :               |                      |    |                      |           :
  Sector1 (32KB)  0x08008000  |----------------------|    |----------------------|  0x08004000 Sector1 (16KB)
                              | flash used for       |    | flash used for       |
                              | implement the EEPROM |    | implement the EEPROM |
                              | emulation mechanism  |    | emulation mechanism  |
                              | (Firmware size: 6KB) |    | (Firmware size: 6KB) |
  Sector0 (32KB)  0x08000000  \----------------------/    \---------------------/   0x08000000 Sector0 (16KB)

@par How to use it ? 

In order to make the program work, you must do the following :
 - Open your preferred toolchain 
 - Rebuild all files and load your image into target memory
 - Run the example

 * <h3><center>&copy; COPYRIGHT STMicroelectronics</center></h3>
 */
