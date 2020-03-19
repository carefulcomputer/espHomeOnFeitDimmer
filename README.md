# espHome for Feit dimmer

This config is for Feit Smart dimmer 
https://www.feit.com/product/smart-wi-fi-dimmer/

Tasmota is not stable (as of now 8.x version). Several times the communication with TuyaMCU completly stops. And several times the webui/mqtt commands do not work. There are other known issue where button presses do not work as expected ( https://community.home-assistant.io/t/costco-feit-smart-dimmer-tuya-convert-tasmota/151390/7?u=carefulcomputer ).

This config will compile espHome which seems stable on these dimmers. Since I do not use HomeAssistant, I was looking for a way to configure this dimmer to be controlled by mqtt (and rest api). This config enables that while disabling any other unneeded modules/communications.

Steps to build/install :
* Install esphome 
  `pip install esphome`
* create a folder and copy the yaml file from this project into that folder.
* modify yaml file to enter name, wifi details, wifi hotspot details (this will be be used when esphome is not able to connect to your wifi as client. This hotspot will allow you to flash new firmware with modified ssid) and mqtt topics
* compile the firmware
 `esphome <yaml_filename>.yaml run`
* you might get syntax error related to UUID package uninstall uuid 
  `pip3 uninstall uuid`
  `pip uninstall uuid`
 
  since it is now included in core python package, you don't need it
* wait until process finishes. It will stay 'success' and point you to location of firmware file which will be of format 
`<name>/.pioenvs/<name>/firmware.bin`
* If you already have tasmota on this device (from tuya-convert) then first disable tasmota compatibility check using
`setoption78 1`
* Now upload firmware.bin on tasmota upgrade page.
* Power cycle the dimmer (if already installed on wall, then you will need to cycle the circuit breaker). This is very important otherwise dimmer will NOT function correctly until power cycle. You need to powercycle after every flash of firmware.

If you had wifi settings correctly configured, esphome should connect to your home wifi. If not it will create a hotspot with configured details allowing you to flash another firmware with correct details. (don't forget to power cycle the switch after modifying)

If everything went correctly you see mqtt messages flowing to your broker. This dimmer allows you to contorl ON/OFF state or brightness either as part of one command or independently.

Here is format of mqtt payload you can send both state and brighness.

`{"state":"ON","brightness":255}`

you can send either of these values independently .

Special thanks to @digiblur and @deadend over at esphome discord channel at https://discordapp.com/channels/530777070435827722/563439987262095361 for patiently helping me in getting to this stage.
 


 
