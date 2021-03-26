# SENTSOR Core ESP32-MICRO
## Introduction
<img src="https://github.com/adamalfath/sentsor-core-esp32micro/blob/main/media/esp32micro-render2.png" width="600">  

"Kecil Kecil Cabe Rawit", an Indonesian proverb which means despite of its small size it's still powerful, and that is what describe SENTSOR Core Board ESP32-MICRO. Bringing latest ESP32 WiFi-BT-BLE SoC with whopping 16MB flash size to store all your firmware and file system; USB to UART bridge with USB Type-C connector for easy development; extremely accurate RTC with onboard rechargeable backup battery; battery-powered capability with full PMIC solution such as charger, protection and monitoring; and four addressable RGB LED for, you know, RGB goodness.

All of that features packed at 2-sided board assembly measured only 35.56x35.56mm. Not only small in size, SENTSOR Core Board ESP32-MICRO also small on power consumption with overall sleep mode power at only XuW, making this board suitable for both general purpose development board and low power embedded application.

## Features
<img src="https://github.com/adamalfath/sentsor-core-esp32micro/blob/main/media/esp32micro-pinoutposter.png" width="600">  

- **Super compact & powerful**, measured only 35.56x35.56mm (1400x1400mil) with 3x10P standard pitch 2.54mm (100mil) pinout, packing all the onboard features.
- **ESP32-WROOM-32UE**, comes with latest [ECO V3](https://www.espressif.com/sites/default/files/documentation/ESP32_ECO_V3_User_Guide__EN.pdfhttps://www.espressif.com/sites/default/files/documentation/ESP32_ECO_V3_User_Guide__EN.pdf) Dual Core 32-bit LX6 microprocessor with clock up to 240MHz, ULP co-processor, 520kB SRAM, 16MB flash.
- **802.11b/g/n WiFi Connectivity**, support mode STA/AP/STA+AP mode with external antenna using U.FL/IPEX connector allowing the board to be installed inside the enclosure without worrying transceiver signal quality. 
- **Bluetooth 4.2 Connectivity**, support classic bluetooth and Low Energy (LE) protocol.
- **CP2104 UART to USB Bridge**, full-speed programmer & data port with auto-reset trigger using DTR and RTS line.
- **DS3231M RTC**, extremely accurate ±5ppm RTC with alarm trigger capability and onboard MS621FE rechargeable backup battery. Connected via I2C at address 0x68.
- **Battery Powered**, run SENTSOR board everywhere without power supply issue with 1-cell Lithium Ion/Polymer battery via JST-PH connector.
- **CN3165 Battery Charger**, solar-powered and USB-compatible Lithium Ion/Polymer charger with on-chip adaptive charging current for wide range of input power scenario.
- **DW01A Battery Protection**, battery usage protection against over-charge, over-discharge, over-current, and short circuit.
- **BQ27441-G1 Battery Fuel Gauge**, accurately monitor your battery Relative State of Charge (RSOC).
- **RT9078 & RT9013 LDO**, dual low quiescent current LDO design providing configurable +3.3V power rail up to 800mA.
- **Super Low Power**, measured sleep mode power consumption down to XuW (XuA XV at VBAT pin, LDO2 off). 
- **Built-in LED/RGB**, single color active-low LED connected to pin IO2, and 4xSK6812 addressable RGB LED connected to pin IO4. SK6812 data out is exposed so you can chain it up with the rest of the LED circuit.
- **MicroSD socket**, connected via SPI with slave select (SS) at pin IO5.
- **22 pin GPIO** @3V3 level, 3xUART, 2xSPI, 2xI2C, 2xI2S, 12bit ADC, 8bit DAC, hall-effect sensing, capacitive sensing, JTAG debug, etc. Please see pinout poster/schematic file for more information.

## How to Use
### Installing USB-UART Driver
Normally the CP2104 driver is automatically recognized and/or installed by Windows Update (for Windows OS). But if you have a problem, you can install it manually by downloading the latest driver here https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers. The step is straight forward, download the file and click the executable file to install the driver.  

After this, you should see the serial communication device (and port number) both on your system (device manager) or in the Arduino IDE software. You can skip this step if you already install the driver.

### Installing ESP32 Hardware Package
Chip from Espressif doesn't supported out-of-the-box by Arduino IDE so we need 3rd-party hardware package called ESP32 Arduino Core. The package installed using board manager, for detailed instruction on how to install ESP32 Arduino Core on Arduino IDE you can follow the step here
https://github.com/espressif/arduino-esp32/blob/master/docs/arduino-ide/boards_manager.md.

Once it's done, ESP32 should be listed on the board selection menu. Choose **ESP32 Dev Module** on board option, **16MB (128Mb)** on the flash size option and set the PSRAM option to disabled. For the partition scheme (application memory & file system) you can choose the allocation combination as needed, and the other option can left as default.  

<img src="https://github.com/adamalfath/sentsor-core-esp32micro/blob/main/media/esp32micro-arduinoideinfo.png" width="600">  

Now your SENTSOR Core ESP32-MICRO is ready to be use! You can skip this step if you already install the ESP32 hardware package.

### Dependency
To access on-board peripheral like RTC, MicroSD & RGB LED you may need to install related library below:
- RTCLib https://github.com/adafruit/RTClib
- SdFat https://github.com/greiman/SdFat
- NeoPixel https://github.com/adafruit/Adafruit_NeoPixel
- BQ27441-G1 https://github.com/sparkfun/SparkFun_BQ27441_Arduino_Library  

or any other library of your choice that have same functionality.

### Battery Powered
SENTSOR Core Board ESP32-MICRO can be powered using 1-cell (3.7V-4.2V) Lithium-Ion/Polymer battery using JST-PH connector. The charging process can be done by supplying external power to VIN pin or USB port, charging current is set via ISET resistor (please see schematic file for more information).

> **:warning: Caution!**  
> Please pay attention to the battery connector polarity, there's +/- sign on the board silkscreen.

### Power Configuration
This board have 2 LDO regulator (named LDO1 & LDO2), where LDO1 is always on and LDO2 controlled using IO27 (active-high). Also there's one exposed jumper pad (labeled VIN CUT, see poster above) which control the VIN input rail selection.

|State|VIN_CUT|VIN|
|-|-|-|
|VBAT > VIN0|X|VBAT|
|VBAT < VIN0|0|VIN0|
|VBAT < VIN0|1|VBAT|

X = don't care. 1 mean the pad is connected, whether using 0Ω resistor or solder bridge. 0 mean the pad is left open (unconnected).

## Bill of Materials
|Designator|Qty|Name/Value|Footprint|
|-|-|-|-|
|U1|1|RT9078-33GJ5|TSOT-23-5_L2.9-W1.6-P0.95|
|U2|1|RT9013-33GB|TSOT-23-5_L2.9-W1.6-P0.95|
|U3|1|CN3165|DFN-8_L3.0-W3.0-P0.50|
|U4|1|DW01A|SOT-23-6_L2.9-W1.6-P0.95|
|U5|1|BQ27441DRZR-G1B|SON-12_L4.0-W2.5-P0.40|
|U6|1|CP2104-F03-GMR|QFN-24_L4.0-W4.0-P0.50|
|U7|1|ESP32-WROOM-32UE(16MB)|WIFIM-32UE-SMD_38P|
|U8|1|DS3231MZ+|SOIC-8_L4.9-W3.9-P1.27|
|R0|1|0 (NC)|R0402|
|R1,R2|2|100k|R0402|
|R10|1|0.01|R0603|
|R11|1|1M|R0402|
|R12,R13|2|5.1k|R0402|
|R23|1|300|R0402|
|R3,R4,R7,R8,R24|5|1k|R0402|
|R5|1|3.9k|R0402|
|R6|1|100|R0603|
|R9,R14,R15,R16,R17, R18,R19,R20,R21,R22|10|10k|R0402|
|C1,C3,C5,C9,C11|5|1u|C0402|
|C10|1|4.7u|C0402|
|C2,C6|2|10u|C0402|
|C4|1|220u, 6.3V|CASE-B_3528|
|C7,C12,C13,C14,C15, C16,C17,C18,C19|9|100n|C0402|
|C8|1|470n|C0402|
|F1|1|BSMD0402L-035|F0402|
|D1,D3|2|B5819WS|SOD-323_L1.8-W1.3|
|D2|1|USBLC6-2SC6|SOT-23-6_L2.9-W1.6-P0.95|
|D4|1|BAS716|SOD-523_L1.2-W0.8|
|LED1|1|Blue|LED0402|
|LED2|1|Green|LED0402|
|LED3|1|Red|LED0402|
|LED4|1|White|LED0402|
|LED5,LED6,LED7,LED8|4|EC20-SK6812|LED2020|
|Q1|1|BSS138PW,115|SOT-323-3_L2.2-W1.3-P1.30|
|Q2|1|HSSK8811|SOT-363_L2.0-W1.3-P0.65|
|Q3|1|SL8205S|SOT-23-6_L2.9-W1.6-P0.95|
|Q4|1|UMH3N|SC-70-6_L2.2-W1.3-P0.65|
|BTN1,BTN2|2|3.9x3x4.45|SW-SMD_L3.9-W3.0-P4.45|
|CN1|1|JST-PH_2P|CONN-SMD_2P-P2.00|
|CN2|1|TYPEC-304S-ACP16|USB-C-SMD|
|CARD1|1|HYC80-TF08-375|HYC80-TF08-XXX|
|BAT1|1|MS621FE|MS621FE|

## Design
<img src="https://github.com/adamalfath/sentsor-core-esp32micro/blob/main/media/esp32micro-pcb-ss.png" width="600">  

SENTSOR Core ESP32-MICRO is an open source hardware, please use it wisely. The project files is hosted on online based EDA called EasyEDA, you can access it via link below:  

Link: https://easyeda.com/sentsor-project/sentsor-core-esp32-micro

## Gallery
_Render View:_  
<img src="https://github.com/adamalfath/sentsor-core-esp32micro/blob/main/media/esp32micro-render0.png" width="400"> <img src="https://github.com/adamalfath/sentsor-core-esp32micro/blob/main/media/esp32micro-render1.png" width="400">  
<img src="https://github.com/adamalfath/sentsor-core-esp32micro/blob/main/media/esp32micro-render2.png" width="400"> <img src="https://github.com/adamalfath/sentsor-core-esp32micro/blob/main/media/esp32micro-render3.png" width="400">

## Support Open Source Hardware & SENTSOR!
If you are interested in our product, you can buy them in the marketplace or by giving donation using this link below:

[![Store](https://img.shields.io/badge/marketplace-Tokopedia-brightgreen.svg)](https://www.tokopedia.com/gerai-sagalarupa/etalase/sentsor-product)  [![Store](https://img.shields.io/badge/marketplace-Tindie-lightgrey.svg)](https://www.tindie.com/)  [![Donate](https://img.shields.io/badge/donate-PayPal-blue.svg)](https://www.paypal.me/adamalfath)  

Your support will be very helpful for the next development of open source hardware from SENTSOR.

## Information
SENTSOR  
Author: Adam Alfath  
Contact: adam.alfath23@gmail.com  
Web: [sentsor.net](http://www.sentsor.net)  
Repo: [SENTSOR Main Repo](http://github.com/adamalfath/sentsor)

<img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/80x15.png"/></a><br/>
SENTSOR Core ESP32-MICRO is licensed under <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.