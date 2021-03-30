# Ender 3 V2

## Version Notes, and Known Bugs

### 0.1

#### BLTouch

* The probe still hangs, the needle fails to deploy.  Source unknown.
* When leveling, the movement is too slow.
* When leveling, the probe doesn't go all the way to the right.  That point is where the nozzle is at the end of the buuld plqte, but there's space on the rail for the head to move all the way for the probe to be at the right edge of the bed.  Not sure how to fix, probably in margins, and dimensions.

#### Endstop

Haven't found any bugs particular to this variant yet.

#### General

* After homing, the nozzle stays in the middle of the bed.  It should park at (0,0,0).  (Might only affect Endstop variant.)
* The version, plate size/ build volume, and other information isn't properly represented in the Information menu page.
* The custom boot screens, and such haven't been included in the build as the files weren't copied to the Marlin directory, just the three text configuration files.

## Installing, Editing, and Building

This distribution relies on VS Code with the Plarformio, and Marlin Auto-Build extensions installed.

Once installed, simply open the Marlin firmware distribution/repository folder in VS Code, along the sample configuration files, or the files from this repository (version 0.1 is based off the sample Marlin configuration files for the Ender 3 V2).  You'll probably want to copy the latest build files in a new folder, incrementing the version number, and edit the machine-specific configuration, and version files for a variant, and start a test build.

Before that, you need to update the platformio.ini file in the Marlin root folder, which should looks like this for the 4.2.2 board:

```
[platformio]
src_dir      = Marlin
boards_dir   = buildroot/share/PlatformIO/boards
default_envs = STM32F103RET6_creality
include_dir  = Marlin
```

Put all the working files in the variant's folder in the Marlin directory at the root of the Marlin firmware distribution/repository, and use Marlin Auto-Build from the left toolbar to build the firmware.  If the build fails, correct the errors, and once you have a successful build, copy the working configuration files back to the working folder, along the firmware itself, and once both variants have successfully built, push the changes to this repo for safekeeping.

## Flashing Firmware

The bootloader which handles flashing new firmware on this board remembers the last filename you used.

Therefore, to flash the compiled firmware binary onto the board you must give the "`firmware.bin`" file on the SD card a unique name, different from the name of the previous firmware file, or you will be greeted with a blank screen on the next boot.

## Updating the Display

To update the graphics and icons on the display:

- Copy the `DWIN_SET` folder to an SD card and insert the card into the slot on the back of the display unit.
- Power on the machine and wait for the screen to change from blue to orange.
- Power off the machine.
- Remove the SD card from the back of the display.
- Power on to confirm a successful flash.
