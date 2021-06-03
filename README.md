# Ender 3 V2

## New Firmware Found

If you look under https://github.com/Jyers/Marlin you'll find a project from Jyers to get all the Marlin functions working on the stock Ender 3 V2 display (the problem is really that the DWIN display is closed-source, but Jyers was able to hack it, and is currently working to get his project upstream of the main Marlin project.

It exists as source, and pre-built binary images, and you'll find the configuration files for those binary releases in the repo; there's releases for UBL, BLTouch, manual, high-speed, various probing densities, etc.  It's a very well-maintained project.

### Instructions for the Jyers firmware

So you just flash whatever release you want, and you configure it from the control panel.  You'll find new configuration options like PID autotune for your hotend, and bed, a mesh viewer, and a slew of other Marlin functions that would not work on the DWIN display drivers Marlin has by default.

If you read his wiki carefully, you'll find the necessary instructions.

And these are my Cura settings for the Ender 3 V2 for UBL, but they should also work with non-UBL probing as well.

````gcode
; Ender 3 Custom Start G-code
G92 E0 ; Reset Extruder
G28 ; Home all axes - G29 disables bed levelling, so the following needs to be after G28.

; Jyers Marlin FW Addition to Enable Levelling
G29 A ; Activate UBL.
G29 L0 ; Load default mesh from EEPROM.
G29 F10 ; Fade corrections over 10 mm (uncompensated (true to printer) above that).
G29 J2 ; Get the bed tilt in a 2 x 2 grid instead of the default 3-points.
M420 S1 Z10 ; Enable leveling/UBL, fade corrections over 10 mm (this is for Non-UBL builds, for compatibility.
; Back to default Cura machine script for the Ender 3 Pro after this

G1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed
G1 X0.1 Y20 Z0.3 F5000.0 ; Move to start position
G1 X0.1 Y200.0 Z0.3 F1500.0 E15 ; Draw the first line
G1 X0.4 Y200.0 Z0.3 F5000.0 ; Move to side a little
G1 X0.4 Y20 Z0.3 F1500.0 E30 ; Draw the second line
G92 E0 ; Reset Extruder
G1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed
G1 X5 Y20 Z0.3 F5000.0 ; Move over to prevent blob squish
````


# This repository is now unmaintained

As I've explained above, I've moved to the Jyers firmware that works **wonders**, it **should** be the firmware that Cura makes for the Ender 3 V2, but it's not.  If you have money, please help him out, he *really* deserves it!

## Possible Future

Since Jyers includes the configuration files for his various builds, it's possible that I tweak some at some point, and if that happens, I'll arrange this repository to serve the purpose of sharing the config files I've modified for the Jyers firmware.

> -
>## --- BELOW IS THE ORIGINAL README WHICH IS REALLY NO LONGER RELEVANT ---
> -

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
