# stm32-stlinkv2-platformio
Quick start guide for PlatformIO and STM32's Chinese Clone (aka BluePill) using Arduino Core on MacOS.

In fact it's not a MacOS-only guide.

# What you need
* STM32 board itself (STM32F103C8 in my case)
* ST-LINK V2
* Hands
* VSCode with PlatformIO

> STLink drivers: https://my.st.com/content/my_st_com/en/products/development-tools/software-development-tools/stm32-software-development-tools/stm32-utilities/stsw-link009.license=1610368232807.product=STSW-LINK009.version=2.0.1.html

# How to get USB Serial working?

https://community.platformio.org/t/difficulty-with-getting-usb-serial-usb-cdc-working/7501/6

> STM32 Virtual COM Port Driver: https://my.st.com/content/my_st_com/en/products/development-tools/software-development-tools/stm32-software-development-tools/stm32-utilities/stsw-stm32102.license=1610355488152.product=STSW-STM32102.version=1.5.0.html#get-software


# Steps
1. Open MacOS terminal: `brew install stlink` (Don't know if this step really even matters 😅)
1. Create a project in PlatformIO
1. **platformio.ini** contents (**This config enables you to use maple core libs.**):
    ```cfg
    [env:bluepill_f103c8]
    platform = ststm32
    framework = arduino
    board = genericSTM32F103C8
    upload_protocol = stlink
    debug_tool = stlink
    upload_flags = -c set CPUTAPID 0x2ba01477
    build_type = debug

    build_flags = 
        -D PIO_FRAMEWORK_ARDUINO_ENABLE_CDC
        -D USBCON
        -D USBD_VID=0x0483
        -D USBD_PID=0x5740
        -D USB_MANUFACTURER="Unknown"
        -D USB_PRODUCT="\"BLUEPILL_F103C8\""
        -D HAL_PCD_MODULE_ENABLED
    ```
    
    > Last line solves error:
    ```
    Warn : UNEXPECTED idcode: 0x2ba01477
    Error: expected 1 of 1: 0x1ba01477
    ```
1. Set both BOOT jumpers of STM32 to zero.
1. Connect ST-LINK V2 to your mac
1. Click upload in PlatformIO
1. Profit

### Let me know if it helped you - star the repo!
