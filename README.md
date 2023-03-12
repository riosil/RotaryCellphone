# Firmware for the RotaryCellphone

Firmware for Justine's original 3G [Rotary Cellphone](https://skysedge.com/unsmartphones/rotarycellphone_3g), not the 4G [Rotary Un-Smartphone](https://skysedge.com/unsmartphones/RUSP/index.html). 

The firmware in the [no-mod](no-mod/main) folder has been endorsed by Justine as official, where *'no-mod'* means hardware modifications are not required.

![Two finished RotaryCellphones](images/two_phones.jpg "Two finished RotaryCellphones")

![Molded leather cases](images/leather_cases.jpg "Molded leather cases")

## Hardware

The FONA Tx cannot talk to the ATmega on pin 19 after the e-paper display is initialised because pin 19 is also the SPI SS pin. The display uses SPI and sets pin 19 to output resulting in two outputs connected together (easily missed).

The solution is to use another ATmega pin for FONA Tx. Digital pin 12 was chosen because it is one of the few ATmega pins that work with Software Serial and because its corner position makes it easier to solder. **The firmware in this folder requires that FONA Tx is re-wired from pin 19 to pin 25.**

For un-modified boards, use the firmware in the [no-mod](no-mod/main) folder.

![Preparing the via](images/hwmod_part1.jpg "Cut the track and prepare the via as a solder pad")

![Re-wire FONA Tx](images/hwmod_part2.jpg "Re-wiring FONA Tx with 0.2mm 'Vero' wire")

FONA Tx rewired to ATmega pin 25 with 0.2mm 'Vero' wire.

#### Vibration motor

It works (where fitted), I have no idea what the original problem was. Comment out `#define HAS_VIBRATE` if the board version does not have space for a vibration motor. This is due to a difference with mode switch wiring between PCB versions. The original PCB (and my clone) has a vibration motor, the kit PCBs do not.

## Firmware

### Official 'no-mod' firmware 
The ATmega SPI SS pin can be reset to input mode after a display update, this works until the next update. The quick hack [here](no-mod-main) resets the SS pin to input and adds a watchdog timer to reset the ATmega. The watchdog reset is only required when the FONA talks to the ATmega during a display update (hopefully not very often).

### Unnoficial firmware

The firmware in this folder only works with the above FONA Tx pin re-wiring mod.

- Changed the name of *main.ino* to *RotaryCellphone.ino* removing the requirement to put everything in a *main* sub-folder.
- Reliably decode FONA messages by parsing AT command responses
- Fix signal strength and battery charge bargraph displays
- Display time from cell network using fast partial update
- Parse and display incoming call ID using fast partial update
- Display graphics on the e-Ink display, slow full refresh (2s)
- Query call state in a 1 second loop to detect hook status
- Get *no-prepend* mode to dial variable length numbers (for non North American numbering schemes)
- Vibration motor works (if installed) when ring level set to zero
- FONA will sleep (but still receive calls) after 5 minutes, doubling battery life to 2 days.
- FONA will wake up on receiving a call or if the 'C' button is held for 1 second.
- Hold the 'C' button for 1s to return to the default screen after a number lookup
- Shutdown the phone when battery voltage drops below 3.3V
- Restart the phone when battery voltage rises above 3.5V (charging)
- When the phone shuts down, the Hook LED flashes once every 4s to indicate charging is required
- Passthrough mode. Hold 'C' while the phone is starting to communicate with the FONA module directly

## Future work

August 2020: no new features are planned for this (now detached) fork, only bug fixes.

I would like a new version of the enclosure bottom so the e-paper display does not have to be bent but I only have the STL files. Editing STL files for this 'organic' shape is almost impossible and I don't have the orginal Inventor files. Even if I did, I'm not an Autodesk Inventor fan, i.e. it's beyond me.

To add features consider putting the 'smartness' upstream, e.g. routing calls through a home PBX based on Asterisk and FreePBX on a Single Board Computer (SBC). Possible features include; call recording, ring groups, diversion, announcements, Interactive Voice Response... you could even make a button on the RotaryCellPhone open a door or turn on driveway lighting:

- [BeagleBone PBX (somewhat disingenuous about current Pi versions)](http://beaglebone-asterisk.raspbx.org/)  
- [Raspberry Pi PBX (low power, very reliable on a Pi3/4 and handles an insane amount of calls)](http:www.raspberry-asterisk.org/)  
- [How Many Calls on Raspberry Pi with Asterisk PBX?](https://www.youtube.com/watch?v=dVGf3HrKZl4)  
- [Call PBX to open a door (skip to 4:50)](https://www.youtube.com/watch?v=kbODHbJyEX4)

It might be possible to use a third party phone module library, some have been considered. TinyGSM is compatible with the SIM5320 but currently not for voice (at the time of writing). The Adafruit FONA library requires the RI (ring indicator) wired to a pin using a specific interupt (another hardware mod would be required).

The leather cases are a skill I learned during lockdown with help from a [video tutorial](https://www.youtube.com/watch?v=lGikTadTN64) by Leodis Leather. The embossing was made with a laser engraved acetal block pressed onto the cased (dampened) leather.