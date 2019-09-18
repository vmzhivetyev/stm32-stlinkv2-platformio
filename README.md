# stm32-stlinkv2-platformio
Quick start guide for PlatformIO and STM32 (aka BluePill) using Arduino Core on MacOS.

In fact it's not a MacOS-only guide.

# What you need
* STM32 board itself (STM32F103C8 in my case)
* ST-LINK V2
* Hands
* VSCode with PlatformIO

# Steps
1. Open MacOS terminal: `brew install stlink` (Don't know if this step really even matters 😅)
1. Create a project in PlatformIO
1. **platformIO.ini** contents:
    ```cfg
    [env:mycustomconfig]
    platform = ststm32
    board = genericSTM32F103C8
    framework = arduino
    upload_protocol = stlink
    debug_tool = stlink
    ```
    **This config enables you to use maple core libs.**
1. Set both BOOT jumpers of STM32 to zero.
1. Connect ST-LINK V2 to your mac
1. Click upload in PlatformIO
1. See the error like this:
    ```log
    hla_swd
    Info : The selected transport took over low-level target control. The results might differ compared to plain JTAG/SWD
    Info : clock speed 1000 kHz
    Info : STLINK V2J29S7 (API v2) VID:PID 0483:3748
    Info : Target voltage: 3.176425
    Warn : UNEXPECTED idcode: 0x2ba01477
    Error: expected 1 of 1: 0x1ba01477
    in procedure 'program'
    ** OpenOCD init failed **
    shutdown command invoked

    *** [upload] Error 1
    ```
1. Now enter following to the terminal inside VSCode: `pio settings set force_verbose Yes`
1. Do upload again
1. Check for the openocd parameters line before the same error in the log. It should look like:
    ```
    openocd -d2 -s /Users/viacheslav/.platformio/packages/tool-openocd/scripts -f interface/stlink.cfg -c "transport select hla_swd" -f target/stm32f1x.cfg -c "program {.pio/build/genericSTM32F103C8/firmware.elf}  verify reset; shutdown;"
    ```
   In the line above you can see the path to opencd’s scripts folder and the subpath (target/stm32f1x.cfg) for the config file used.
   Open the config file and replace the `0x1ba01477` with `0x2ba01477`.
1. Run upload again.
1. ??????
1. Profit
