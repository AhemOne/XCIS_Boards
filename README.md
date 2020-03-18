# XCIS Boards
This repository contains support for XCIS Arduino-like board variants.

#### AVR Boards

* [Node](https://www.xcis.com.au)
* [Device](https://www.xcis.com.au)

#### ESP8266 Boards
_These boards are supported by [Espressif](https://github.com/esp8266/Arduino)_.
* [Gateway](https://www.xcis.com.au)

### Installation Instructions

Start Arduino and open the Preferences window (**File** > **Preferences**). Copy and paste the following URL into the 'Additional Boards Manager URLs' input field:

	https://raw.githubusercontent.com/sparkfun/Arduino_Boards/master/IDE_Board_Manager/package_sparkfun_index.json


### AVR Installation Instructions

Open the Boards Manager window by selecting **Tools** > **Board**, scroll to the top of the board list, and select **Boards Manager**.

Ttype "xcis" (without quotes) into the "filter your search" field. Click in the desired box, and click the "Install" button that appears. Once installed, the boards will appear at the bottom of the board list.

### ESP8266 Boards

All support for our ESP based boards are supported within their respective Espressif's Repository. 

* Follow the instructions [here](https://github.com/esp8266/Arduino).

### Notes

* This was copy and pasted from Sparkfun's repository and should probably be rewritten.
