
# Change log for JoyShockMapper
Most recent updates will appear first.
This is a summary of new features and bugfixes. Read the README to learn how to use the features mentioned here.

## 1.6.1
Lots of internal changes for developers. JSM can now be built for Linux, thanks to Romeo Calota. Since this is only developer-facing for now, this is still just a bug-fix update rather than a feature update. But if you're up for it, check out the Linux instructions in the README!

Regarding bugs, Nicolas fixed some bugs with disabling gyro and stick behaviour during mode shifts. Jibb added support for wired Switch Pro Controller (technically a new feature but the lack of support was a stumbling block for many new users) and made some changes to the DualShock 4 Bluetooth support that will hopefully fix issues some users have been having.

### Bugfixes
* Improved support for DualShock 4 and Switch Pro Controller.
* Fixed an issue with stick behaviour and mode shift.
* Fixed d-pad up always acting as a gyro off button even when it wasn't assigned.

## 1.6.0
Nicolas added modeshifts to JSM and all setting variables are now encapsulated in a structure. Therefore, to use any setting you need to query it through the accessor in Joyshock. All settings are now "optional" but should always be set in the base structure by the reset() function. Instances that are part of the modeshift map can have nullopt to indicate no alternate value for the setting when that modeshift is active. Nicolas also added MOUSE_AREA for a stick mode, which is useful for using mouse wheels in game where the wheel is not centered on the screen (in which case MOUSE_RING would work). See README for details.

### Features
* Added the ability to chord any setting (except autoload, and including NO_GYRO_BUTTON).
* Modeshifts are removed by assigning NONE.
* Added MOUSE_AREA as a stick mode.

## 1.5.1
Nicolas changed the tray icon to always be displayed. 

### Bugfixes
* The one second pause when exiting JSM was not necessary.

## 1.5.0
Nicolas added double press bindings, improved chorded mapping behaviour when combined with taps and holds, and refactored a lot of code to prepare for some future changes. Also added support for mouse buttons forward and back, separate horizontal and vertical gyro sensitivities, and changed the way logs are displayed. Jibb added new ways to configue flick stick: customise flick stick's smoothing, disable some of its features, or snap to angles. Also made it so ring bindings work alongside any stick mode and added the MOUSE\_RING stick mode for 2D twin-stick aiming.

### Features
* Added ability to assign Double Press mappings to a button, by entering the button chorded with itself (eg: S,S = SPACE).
* Chords are now active when the controller button is down, instead of waiting for a bounded input to be resolved (such as taps and holds).
* Support mouse buttons 4 and 5 (back and forward).
* Horizontal and vertical gyro sensitivities can now be set independently by including a second number for the Y sensitivity (first will be used for X).
* Added MOUSE\_RING stick mode to let you use the stick to point the mouse in a direction relative to the centre of the screen.
* Added new stick modes enabling only the flick (FLICK\_ONLY) or rotation (ROTATE\_ONLY) with flick stick.
* Added FLICK\_SNAP\_MODE and FLICK\_SNAP\_STRENGTH for those who'd prefer flick stick snapped to cardinal (or intercardinal) directions with the initial flick.
* LEFT\_RING\_MODE and RIGHT\_RING\_MODE = INNER/OUTER allows setting ring bindings regardless of what LEFT_STICK_MODE and RIGHT_STICK_MODE are.
* Added the option to override the smoothing threshold for flick stick with ROTATE\_SMOOTH\_OVERRIDE. The smoothing window is still small, but it might soften things for those who found Switch sticks too twitchy.
* Change some behind the scene mapping of commands to windows virtual key codes.

## 1.4.2
1.4.2 is a bugfix update. Nicolas fixed a crash and combo (aka Simultaneous and Chorded) presses not clearing properly, as well as some under-the-hood tweaks. Jibb tweaked communication with Bluetooth DualShock 4.

### Bugfixes
* Fixed crash when left clicking the tray icon.
* RESET\_MAPPINGS should now clear combo mappings.
* Setting combo presses to NONE should now clear previous bindings.
* Changes to how Bluetooth works with DualShock 4 controllers might fix issues some had with gyro not working.

## 1.4.1
1.4.1 is a bugfix update, with some minor features as well. Jibb improved flick stick's behaviour when the stick is released slowly, added the BACKSPACE mapping, and made the list of available configs update whenever the tray menu is re-opened. Nicolas added a "Whitelist" toggle to the tray menu.

### Features
* Added BACKSPACE mapping.
* Added buffer between flick stick activation threshold and release threshold.
* Added "Whitelist" toggle to tray menu.
* Tray menu items are refreshed whenever the menu is re-opened.

### Bugfixes
* Fixed combined tap mappings on ZL and ZR not releasing correctly.
* Fixed GYRO_INVERT overlapping ENTER keyboard mapping.
* Fixed outer ring bindings not working properly.
* Configs shortcuts in tray menu now use relative paths instead of absolute paths.
* Minor fixes to the console text.
* Fixed stick directional mappings to buttons not working as they should.

## 1.4.0
In 1.4, Nicolas added simultaneous and chorded press mappings, ring bindings, and options to map a button to inverting gyro input. He also added a HELP shortcut to the latest version of the README and a tray icon (created by Bryan and coloured by Al) that gives easy access to configs and useful commands when JSM is minimised. Jibb added Bluetooth support for the DualShock 4.

### Features
* Simultaneous press - map a pair of inputs pressed at about the same time to a unique output.
* Chorded press - change the mappings of one or more buttons while a particular other button is pressed.
* Bluetooth support for the DualShock 4.
* Ring bindings - have a virtual input apply when either stick is fully pressed or only partially pressed, such as for walking or sprinting.
* Invert gyro - have the gyro mouse inverted (in both axes or one axis of your choice) while pressing a button.
* GYRO\_OFF, GYRO\_ON can appear on the right hand side of regular mappings for combining them with other inputs in interesting ways. Gyro-related mappings bound to a button tap will apply for 0.5s to give them time to be useful.
* Whitelisting - add or remove JoyShockMapper to or from the HIDCerberus whitelist, if it's installed.
* Tray icon - when minimised, JoyShockMapper has a tray icon that can be right-clicked to quickly access configs or useful commands.
* The HELP command will open the README.

## 1.3.0
In 1.3, Nicolas added AutoLoad and dual stage triggers, while Jibb fixed a couple of bugs.

### Features
* AutoLoad - automatically load the appropriate config when an application comes into focus.
* Dual Stage Triggers - map soft presses and full presses of the DualShock 4's triggers to different outputs.

### Bugfixes
* Fixed SL and SR not working properly on Joy-Cons.
* Fixed hold NONE mappings not working.

## 1.2.0
In 1.2, Jibb added features to help with single Joy-Con control as well as more keyboard mappings and better comment support.

### Features
* Gyro axes can be mapped to different mouse axes using MOUSE\_X\_FROM\_GYRO\_AXIS and MOUSE\_Y\_FROM\_GYRO\_AXIS.
* HOME and CAPTURE can now be mapped to any output. Any button can be mapped to CALIBRATE.
* PAGEUP, PAGEDOWN, HOME, END, INSERT, and DELETE keyboard mappings were added.
* /# comments can be added at the end of a line, instead of requiring their own line.

## 1.1.0
In 1.1, Jibb added more ways to enable or disable the gyro, changed the default behaviour of calibration, and fixed a couple of bugs.

### Features
* GYRO\_OFF and GYRO\_ON can be set to LEFT\_STICK or RIGHT\_STICK so that the gyro can be enabled or disabled depending on whether a given stick is being used.
* Continuous calibration is now disabled when a device is first connected, since sometimes devices don't need to be calibrated on startup.

### Bugfixes
* Fixed d-pad up being the gyro off button when none was set.
* Fixed a crash when a bad command was entered.

## 1.0.2
1.0.2 is a bugfix update.

### Bugfixes
* Fixed a bug where arrow keys couldn't be mapped properly.

## 1.0.1
1.0.1 is a bugfix update.

### Bugfixes
* Statically linked runtime so that users don't have to have any particular MSVC runtimes installed.

## 1.0
JoyShockMapper 1.0 was the first public release of JoyShockMapper, created by Jibb Smart. Its features are too many to list in the changelog, but explore the README to see what it offers!
