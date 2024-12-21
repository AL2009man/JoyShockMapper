# JoyShockMapper
The Sony PlayStation DualSense, DualShock 4, Nintendo Switch JoyCons (used in pairs), and Nintendo Switch Pro Controller have much in common. They have many of the features expected of modern game controllers. They also have an incredibly versatile and underutilised input that their biggest rival (Microsoft's Xbox One controller) doesn't have: a 3-axis gyroscope (from here on, “gyro”).

My goal with JoyShockMapper is to enable you to play PC games with DS, DS4, JoyCons, and Pro Controllers even better than you can on their respective consoles -- and demonstrate that more games should use these features in these ways.

**Download JoyShockMapper to use right away [here](https://github.com/Electronicks/JoyShockMapper/releases)**!

For developers, version 2.2 and older serve as a reference implementation for using [JoyShockLibrary](https://github.com/jibbsmart/JoyShockLibrary) to read inputs from DualShock 4, DualSense, JoyCons, and Pro Controller in your games. It now uses [SDL2](https://github.com/libsdl-org/SDL) for controller support, and I've made code contributions to SDL2 to make sure it covers the same features.

JoyShockMapper is also a reference implementation for many of the best practices described on [GyroWiki](http://gyrowiki.jibbsmart.com).

JoyShockMapper is primarily developed on Windows. JoyShockMapper should now be able to be built on and for Linux. See the instructions for that below. Please let us know if you have any trouble with this.

## Contents
* **[Installation for Devs](#installation-for-devs)**
  * **[Linux specific notes](#linux-specific-notes)**
* **[Installation for Players](#installation-for-players)**
* **[Quick Start](#quick-start)**
* **[Configuration Files](#configuration-files)**
  * **[OnStartup.txt](#1-onstartuptxt)**
  * **[OnReset.txt](#2-onresettxt)**
  * **[Autoload feature](#3-autoload-feature)**
* **[Troubleshooting](#troubleshooting)**
* **[Known and Perceived Issues](#known-and-perceived-issues)**
* **[Credits](#credits)**
* **[Helpful Resources](#helpful-resources)**
* **[License](#license)**

## Installation for Devs
JoyShockMapper is written in C++ and is built using CMake.

The project is structured into a set of platform-agnostic headers, while platform-specific source files can be found in their respective subdirectories.
The following files are platform-agnostic:
1. ```include/JoyShockMapper.h``` - This header provides important type definitions that can be shared across the whole project. No variables are defined here, only constants.
2. ```include/InputHelpers.h``` - This is platform agnostic declaration of wrappers for OS function calls and features.
3. ```include/PlatformDefinitions.h``` - This is a set of declarations that create a common ground when dealing with platform-specific types and definitions.
4. ```include/TrayIcon.h``` - This is a self contained module used to display in Windows an icon in the system tray with a contextual menu.
5. ```include/Whitelister.h``` - This is another self contained Windows specific module that uses a socket to communicate with HIDCerberus and whitelist JSM, the Linux implementation, currently, is a stub.
6. ```include/CmdRegistry.h``` - This header defines the base command type and the CmdRegistry class that processes them.
7. ```include/JSMAssignment.hpp``` - Header for the templated class assignment commands
8. ```include/JSMVariable.hpp``` - Header for the templated core variable class and a few derivatives.
9. ```src/main.cpp``` - This does just about all the main logic of the application. The core processing logic should be kept in the other files as much as possible, and have the JSM specific logic in this file.
10. ```src/CmdRegistry.cpp``` - Implementation for the command line processing entry point.
11. ```src/operators.cpp``` - Implementation of all streaming and comparison operators for custom types declared in ```JoyShockMapper.h```

The Windows implementation can be found in the following files:
1. ```src/win32/InputHelpers.cpp```
2. ```src/win32/PlatformDefinitions.cpp```
3. ```src/win32/Whitelister.cpp.cpp```
4. ```include/win32/WindowsTrayIcon.h```
5. ```src/win32/WindowsTrayIcon.cpp```

The Linux implementation can be found in the following files:
1. ```src/linux/InputHelpers.cpp```
2. ```src/linux/PlatformDefinitions.cpp```
3. ```src/linux/Whitelister.cpp.cpp```
4. ```include/linux/StatusNotifierItem.h```
5. ```src/linux/StatusNotifierItem.cpp```

Generate the project by runnning the following in a command prompt at the project root:
- Windows:
  * ```mkdir build && cd build```
  * To create a Visual Studio x86 configuration: ```cmake .. -G "Visual Studio 16 2019" -A Win32 .```
  * To create a Visual Studio x64 configuration: ```cmake .. -G "Visual Studio 16 2019" -A x64 .```
- Linux:
  * ```mkdir build && cd build```
  * ```cmake .. -DCMAKE_CXX_COMPILER=clang++ && cmake --build .```

### Linux specific notes
Please note that JoyShockMapper is primarily written for Windows and is a program in rapid development.

While JSM can be built for Linux, please note that the rapid pace of development and limited number of Linux maintainers means that the Linux release may not always build.

In order to build on Linux, the following dependencies must be met, with their respective development packages:
- clang++ (see below bug)
- gtk+3
- libappindicator3
- libevdev
- libusb
- SDL2
- hidapi

**Distribution-Specific Package Names:**

* Fedora: ```clang SDL2-devel libappindicator-gtk3-devel libevdev-devel gtk3-devel libusb-devel hidapi-devel```
* Arch: ```clang sdl2 libappindicator-gtk3 libevdev gtk3 libusb hidapi```
* Please provide an issue report or pull request to have additional library lists added.

Due to a [bug](https://stackoverflow.com/questions/49707184/explicit-specialization-in-non-namespace-scope-does-not-compile-in-gcc) in GCC, the project in its current form will only build with Clang.

JoyShockMapper was initially developed for Windows, this has the side-effect of some types used through the code-base are Windows specific. These have been redefined on Linux. This was done to keep changes to the core logic of the application to a minimum, and lower the chance of causing regressions for existing users.

The application requires ```rw``` access to ```/dev/uinput```, and ```/dev/hidraw[0-n]``` (the actual device depends on the node allocated by the OS). This can be achieved by ```chown```-ing the required device nodes to the user running the application, or by applying the udev rules found in ```dist/linux/50-joyshockmapper.rules```, adding your user to the input group, and restarting the computer for the changes to take effect. More info on udev rules can be found at https://wiki.archlinux.org/index.php/Udev#About_udev_rules.

The application will work on both X11 and Wayland, though focused window detection only works on X11.

## Installation for Players
The latest version of JoyShockMapper can always be found [here](https://github.com/Electronicks/JoyShockMapper/releases). All you have to do is run JoyShockMapper.exe.

Included is a folder called GyroConfigs. This includes templates for creating new configurations for 2D and 3D games, and configuration files that include the settings used for simple [Real World Calibration](#5-real-world-calibration).

## Quick Start
1. Connect your controller either with a usb cable or via bluetooth. Most modern controllers will be suported, including all Xbox, Playstation and Switch controllers, although Xbox and many others don't have the gyro sensor required for gyro controls.

2. Run the JoyShockMapper executable, and you should see a console window welcoming you to JoyShockMapper.
    * In the console you can start entering bindings : [button name] = [key name]. See [Digital Inputs section](#1-digital-inputs) for details on how buttons and keys are named.
    * [Sticks](#3-stick-configuration), the [gyro](#4-gyro-mouse-inputs) and [analog triggers](#2-analog-triggers) require some more configuration: typically some MODE you want to set, a sensitivity value and some other settings. Each is explained in the corresponding section. They follow the same format : [setting name] = [value]
	* Buttons and settings will display their current values if you only enter their name in the console.
	* Settings can display a short description of what they do if you enter [setting name] HELP
	* There are quite a few commands that do not work as assignments like above but just runs a function. For example RECONNECT\_CONTROLLERS will update the controller listing, and RESET\_MAPPINGS will set all settings and bindings to default. README will lead you to this document!
	* You will find a JoyShockMapper icon in the system tray: right click on it to display a quick list of commands and configuration files.

3. JoyShockMapper can load all the commands contained in a text file.
	* Enter the file's path and name in the console. If the file path is too long, or contains unusual characters, enter the relative path instead (eg: GyroConfigs/config.txt).
    * Every line of the text file will be run as a command typed directly into the console. The pound # symbol starts a comment until the end of the line and will be ignored by JoyShockMapper.
    * You can find example configuration files in the GyroConfigs folder. Files can refer one another : feel free to edit those in GyroConfigs as your customized gyro configuration for example.
    * Find more details in the [Configuration Files](#configuration-files) section.

4. If you're using a configuration that utilises gyro controls, the gyro will need to be calibrated (to be told what "not moving" is). See [Gyro Mouse Inputs section](#4-gyro-mouse-inputs) for more info on that, but here's the short version:
	* Put all controllers down on a still surface;
	* Enter the command RESTART\_GYRO\_CALIBRATION to begin calibrating them;
	* After just a couple of seconds, enter the command FINISH\_GYRO\_CALIBRATION to finish calibrating them.
	* These commands are also accessible via the tray icon contextual menu as well.
    * JoyShockMapper relies on a Real World Calibration value for some features such as flick stick. If you didn't find this value in the [online database](http://gyrowiki.jibbsmart.com/games), check the [Real World Calibration](#5-real-world-calibration) section to calculate it yourself.

5. If you run into some issues, make sure you check the [Troubleshooting](#troubleshooting) section and [Known and Perceived Issues](#known-and-perceived-issues). If you couldn't find your answer, you can find more help online on the [GyroGaming subreddit](https://www.reddit.com/r/GyroGaming/) and its [affiliated Discord Server](https://discord.gg/4w7pCqj).

## Configuration Files

All of the commands layed out in the previous section can be saved in a text file and run all at once. In Windows, you can also drag and drop a file from Explorer into the JoyShockMapper console window to enter the full path of that file. These configuration files can additionally reference one another. This allows you to group a few settings as a "building block" for your configurations: such as your gyro sensitivity and acceleration preferences.

If you enter a relative path to the file, it should be relative to the folder where JoyShockMapper.exe is located. If however your files don't seem to get picked up, you can manually set where to look for the configuration files by entering the command ```JSM_DIRECTORY = D:\JSM``` for example. You can also set that working directory as a command line argument when running JoyShockMapper, which can be done in a shortcut properties. Putting all your configuration files in a synchronized folder allows you to have those configurations across all computers you use for gaming!

What more? There are some configuration files that can be run automatically to streamline your experience.

### 1. OnStartup.txt

When JoyShockMapper first boots up, it will attempt to load the commands found in the file OnStartup.txt. This file should be in the JSM_DIRECTORY, which is next to your executable by default. This is a great place to automatically calibrate the gyro, load a default configuration for navigating the OS, and/or whitelisting JoyShockMapper.

### 2. OnReset.txt

This configuration is found in the same location as OnStartup.txt explained above. This file is run each time RESET\_MAPPINGS is called, as well as before OnStartup.txt. This file is a good spot to set a CALIBRATE button for your controller and/or set your GYRO\_SPACE if you're not using the default value.

### 3. Autoload feature

JoyShockMapper can automatically load a configuration file for your games each time the game window enters focus. Drop the file in the **AutoLoad** folder where JSM_DIRECTORY refers to. JoyShockMapper will look for a name based on the executable name of the program that's in focus. When it goes into focus and autoload is enabled (which it is by default), JoyShockMapper will tell you the name of the file it's looking for - case insensitive.

This enables the user to swap focus between your text editor of choice and the game, and each time the configuration will be automatically reloaded with your latest edits (assuming you've saved!). This system also avoids to step on your toes by **NOT** reloading your configuration if you do change focus between JoyShockMapper itself and the game: any mappings you enter by hand won't be thrown away when you return to your game.

Autoload can be turned off by entering the command ```AUTOLOAD = OFF```. You can enable it again with ```AUTOLOAD = ON```.


## Troubleshooting
Some third-party devices that work as controllers on Switch, PS4, or PS5 may not work with JoyShockMapper. It only _officially_ supports first-party controllers. Issues may still arise with those, though. Reach out, and hopefully we can figure out where the problem is.

But first, here are some common problems that are worth checking first.

* In some circumstances, the JoyShockMapper console is responding to controller input and the mouse is responding to gyro movements, but the game you're playing isn't responding to it. This can happen when you launch the game (or it's launcher) as an administrator. JoyShockMapper must also be launched with administrator rights in order to send keyboard and mouse events to the game. Windows shortcuts can be set to always run as admininstrator in the properties window.

* The JoyShockMapper console will tell you how many devices are connected, and will output information with most inputs (button presses or releases, tilting the stick). However, the only way to test that the gyro is working is to enable it and see if you can move the mouse. The quickest way to check if gyro input is working without loading a config is to just enter the command ```GYRO_SENS = 1``` and then move the controller. Don't forget that you might need to calibrate the gyro if the mouse is moving even when the controller isn't.

* Many users of JoyShockMapper rely on tools like HIDGuardian to hide controller input from the game. If JSM isn't recognising your controller, maybe you haven't whitelisted JoyShockMapper. Enter WHITELIST_ADD to do so. You can also add this command to your OnStartup.txt script to do it everytime.

* Some users have found stick inputs to be unresponsive in one or more directions. This can happen if the stick isn't using enough of the range available to it. In this case, increasing STICK\_DEADZONE\_OUTER can help. In the same way, the stick might be reporting a direction as pressed even when it's not touched. This happens when STICK\_DEADZONE\_INNER is too small.

## Known and Perceived Issues

### Bluetooth connectivity
JoyCons and Pro Controllers normally only communicate by Bluetooth. Some Bluetooth adapters can't keep up with these devices, resulting in **laggy input**. This is especially common when more than one device is connected (such as when using a pair of JoyCons). There is nothing JoyShockMapper or JoyShockLibrary can do about this. JoyShockMapper experimentally supports connecting Switch controllers by USB.

## Credits
JoyShockMapper was originally created by **Julian "Jibb" Smart**. As of version 1.3, JoyShockMapper has benefited from substantial community contributions. Huge thanks to the following contributors:
* Nicolas (code)
* Bryan Rumsey (icon art)
* Contributer (icon art)
* Sunny Ye (translation)
* Romeo Calota (linux and general portability)
* Garrett (code)
* Robin (linux and controller support)

As of version 3, JoyShockMapper development is lead by **Nicolas Lessard**, who was already a long-time contributor and responsible for many of JoyShockMapper's powerful mapping features, autoload, tray menus, and much more. Have a look at the CHANGELOG for a better idea of who contributed what. While Jibb continues on as a contributor, JoyShockMapper is Nicolas' project now. This means updates won't be bottlenecked by Jibb's availability to approve and build them, and Nicolas has final say on what features are included in new versions. As such, make sure you're on [Nicolas' fork](https://github.com/Electronicks/JoyShockMapper) for the latest developments.

JoyShockMapper versions 2.2 and earlier relied a lot on Jibb's [JoyShockLibrary](https://github.com/jibbsmart/JoyShockLibrary), which it used to read controller inputs. Newer versions use [SDL2](https://github.com/libsdl-org/SDL) to read from controllers, as the latest versions of SDL2 are able to read gyro and accelerometer input on the same controllers that could already be used with JoyShockLibrary, but also support many non-gyro controllers as well.

Since moving to SDL2, JoyShockMapper uses Jibb's [GamepadMotionHelpers](https://github.com/JibbSmart/GamepadMotionHelpers), a small project that provides the sensor fusion and calibration options of JoyShockLibrary without all the device-specific stuff.

## Helpful Resources
* [GyroWiki](http://gyrowiki.jibbsmart.com) - All about good gyro controls for games:
  * Why gyro controls make gaming better;
  * How developers can do a better job implementing gyro controls;
  * How to use JoyShockMapper;
  * User editable collection of user configurations and tips for using JoyShockMapper with a bunch of games.
* [GyroGaming subreddit](https://www.reddit.com/r/GyroGaming/)
* [GyroGaming discord server](https://discord.gg/4w7pCqj).

## License
JoyShockMapper is licensed under the MIT License - see [LICENSE.md](LICENSE.md).
