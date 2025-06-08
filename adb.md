
<!-- TOC -->

- [General](#general)
- [Properties](#properties)
- [File I/O](#file-io)
- [dumpsys](#dumpsys)
- [Battery manipulation](#battery-manipulation)
- [Adb shell pm](#adb-shell-pm)
- [Adb shell ps](#adb-shell-ps)
- [Adb shell pwd](#adb-shell-pwd)
- [Adb shell mkdir](#adb-shell-mkdir)
- [Adb shell rm](#adb-shell-rm)
- [Adb logcat](#adb-logcat)
    - [Customization of the log](#customization-of-the-log)
    - [Viewing log of different buffers](#viewing-log-of-different-buffers)
    - [Viewing log of different levels](#viewing-log-of-different-levels)
    - [Change buffer length](#change-buffer-length)
- [APK install](#apk-install)
- [Flashing](#flashing)
- [Record/Capture](#recordcapture)
- [Adb shell top](#adb-shell-top)
- [Permissions](#permissions)
- [Package Info](#package-info)
- [ndk-build](#ndk-build)
- [Misc](#misc)
- [Switching to usb mode for adb](#switching-to-usb-mode-for-adb)
    - [To find touch events](#to-find-touch-events)
    - [Sending input events from adb](#sending-input-events-from-adb)

<!-- /TOC -->


# General
- `adb help` - List available commands
- `adb devices` - lists connected devices
- `adb start-server` - Start the adb server
- `adb kill-server` - Kill the adb server
- `adb root` - Restart adb with root permissions
- `adb reboot` - Reboot the device
- `adb devices -l` - List model info of the devices too
- `adb shell` - Start the backround terminal
- `adb -e <command>` - Send to the emulator
- `adb -d <command>` - Send command to direct attached file
- `adb -s <deviceName> <command>` - Send command to a specific device
- Working with multiple devices
    - Windows
    ```bash
    set ANDROID_SERIAL=7f1c864e
    echo %ANDROID_SERIAL%
    ```

    - Linux
    ```bash
    export ANDROID_SERIAL=R5CT109JYBH
    echo $ANDROID_SERIAL
    ```

# Properties
- `adb shell getprop` - List all available properties and their values
- `adb shell getprop -T` List all available properties and their type
- `adb shell getprop x.y.z` - Get value of a specific property
- `adb shell setprop x.y.z 2` - Set value of a specific property
- `adb shell resetprop --delete x.y.z` - Reset the property
- Get list of different sets of props
    ```bash
    adb shell settings list global
    adb shell settings list system
    adb shell settings list secure
    ```

# File I/O
- `adb push /path/on/local /path/on/device` - Copy file/folder to the device
    - `--sync` -- Only transfer file/folder which are newer
    - `-z` -- Enable compression
    - `-Z` -- Disable compression

- `adb pull /path/on/device /path/on/local` - Copy file/folder from the device
    - `-a` -- preserve file timestamp and mode
    - `-z` -- Enable compression
    - `-Z` -- Disable compression

# dumpsys
- `adb shell dumpsys` - Dump everthing collected by dumpsys
- `adb shell dumpsys SurfaceFlinger` - Dump everthing collected about SurfaceFlinger
- `adb shell dumpsys -l` - Only list services
- `adb shell dumpsys battery` - Gets device battery info
- `adb shell dumpsys package packages` - Get info of all apps
- `adb shell dumpsys activity <package>/<activity>` - activity info
- `adb shell dumpsys package packages` - list info on all apps
- `adb shell dumpsys battery set level <n>` - change the level from 0 to 100
- `adb shell dumpsys battery set status<n>` - change the level to unknown, charging, discharging, not charging or full
- `adb shell dumpsys battery reset` - reset the battery
- `adb shell dumpsys battery set usb <n>` - change the status of USB connection. ON or OFF

# Battery manipulation
- Change the battery level
    ```bash
    adb shell dumpsys battery set level 65
    adb shell dumpsys battery set status 0|1|2|3
    ```

- Change the status to unknown, charging, discharging, not charging or full.
    ```bash
    adb shell dumpsys battery set status 3
    adb shell dumpsys battery reset
    ```

- Reset the battery
    ```bash
    adb shell dumpsys battery reset
    adb shell dumpsys battery set usb 0|1
    ```

- Change the status of USB connection to On/Off
    ```bash
    adb shell dumpsys battery set usb 0
    adb shell dumpsys
    ```

# Adb shell pm
This command is used to call package manager and do specific task which only package manager can do

- `adb shell pm clear com.hello.world` - Deletes all data associated with a package.
- `adb shell pm uninstall com.hello.world` - Removes a package from the system.
- `adb shell pm list features` - List all phone features
- `adb shell pm list libraries` - Prints all the libraries supported by the current device.
- `adb shell pm list packages --apex-only` - Only show APEX packages
- `adb shell pm list packages --show-versioncode` - Also show the version codea
- `adb shell pm list packages --uid UID` - Filter to only show packages with the given UID
- `adb shell pm list packages --user USER_ID` - Only list packages belonging to the given user
- `adb shell pm list packages -3` - Filter to only show third party packages
- `adb shell pm list packages -a` - All known packages but excluding APEXes
- `adb shell pm list packages -d` - Filter to only show disabled packages
- `adb shell pm list packages -e` - Filter to only show enabled packages
- `adb shell pm list packages -f` - See their associated APK file
- `adb shell pm list packages -i` - See the installer for the packages
- `adb shell pm list packages -l` - Ignored (used for compatibility with older releases)
- `adb shell pm list packages -s` - Filter to only show system packages
- `adb shell pm list packages -u` - Also include uninstalled packages
- `adb shell pm list packages -U` - Also show the package UID
- `adb shell pm list packages` - Prints all packages
- `adb shell pm list permission-groups` - Prints all known permission groups.
- `adb shell pm list users` - Prints all users on the system.
- `adb shell pm path com.hello.world` - Display the path of package

# Adb shell ps
This command is used to show the list of all processes in android

- `adb shell ps -A` - Show all processes that aren't session leaders
- `adb shell ps -p PID` - Show a process with the particular PID
- `adb shell ps -p 1256` - Filter PIDs (`--pid`)
- `adb shell ps -t` - Show threads
- `adb shell ps -d` - Show all processes that aren't session leaders
- `adb shell ps` - List processes. Which processes to show (selections may be comma separated lists)

# Adb shell pwd
- `adb shell pwd` - This command is used to print the current working directory

# Adb shell mkdir
`mkdir -m 777 /path/to/directory` - Sets up permission mode
`mkdir -p /path/to/directory` - Create parent directories as needed

# Adb shell rm
- `adb shell rm -f /path/to/directory` - Forcefully remove without asking for confirmation. Won't throw any error if the file does not exist.
- `adb shell rm -i /path/to/directory` - Interactively prompt for confirmation.
- `adb shell rm -rR /path/to/directory` - Recursively remove directory contents.
- `adb shell rm -v /path/to/directory` - Verbose.
- `adb shell rm /path/to/directory` -

# Adb logcat

Prints log data to the screen.
```bash
adb logcat [option] [filter-specs]
adb logcat
```

> Notes: press Ctrl-C to stop monitor

## Customization of the log
- `adb logcat -s` - Sets the default filter spec to silent.
- `adb logcat -v color` - show colorful logcat
- `adb logcat -v <format>` - Format
- `adb logcat -v brief` - Display priority/tag and PID of the process issuing the message (default format).
- `adb logcat -v process` - Display PID only.
- `adb logcat -v tag` - Display the priority/tag only.
- `adb logcat -v raw` - Display the raw log message, with no other metadata fields.
- `adb logcat -v time` - Display the date, invocation time, priority/tag, and PID of the process issuing the message.
- `adb logcat -v threadtime` -  Display the date, invocation time, priority, tag, and the PID and TID of the thread issuing the message.
- `adb logcat -v long` - Display all metadata fields and separate messages with blank lines.
- `adb bugreport` - print bug reports
- `adb logcat -c` - clear only non-rooted buffers
- `adb logcat -f test.logs` - Writes log message output to test.logs
- `adb logcat -n <count>` - Sets the maximum number of rotated logs to `<count>`.  Ntes: The default value is 4. Requires the -r option.
- `adb logcat -r <kbytes>` - Rotates the log file every `<kbytes>` of output. Notes: The default value is 16. Requires the -f option.

## Viewing log of different buffers
- `adb logcat -b all` - Everything
- `adb logcat -b crash` - Get crashlog
- `adb logcat -b radio ` - View the buffer that contains radio/telephony related messages.
- `adb logcat -b event` - View the buffer containing events-related messages.
- `adb logcat -b main` - Default
- `adb logcat -b kernel` - Kernel buffer
- `adb logcat -b system` - system buffer

## Viewing log of different levels
- `adb logcat *:V` - Lowest priority, filter to only show Verbose level
- `adb logcat *:D` - Filter to only show Debug level
- `adb logcat *:I` - Filter to only show Info level
- `adb logcat *:W` - Filter to only show Warning level
- `adb logcat *:E` - Filter to only show Error level
- `adb logcat *:F` - Filter to only show Fatal level
- `adb logcat *:S` - Silent, highest priority, on which nothing is ever printed

## Change buffer length
- `adb logcat -g` - see default buffer lengths

```text
- main: ring buffer is 5 MiB (1 MiB consumed, 883 KiB readable), max entry is 5120 B, max payload is 4068 B
- system: ring buffer is 2 MiB (512 KiB consumed, 180 KiB readable), max entry is 5120 B, max payload is 4068 B
- crash: ring buffer is 512 KiB (128 KiB consumed, 261 B readable), max entry is 5120 B, max payload is 4068 B
- kernel: ring buffer is 4 MiB (1 MiB consumed, 448 KiB readable), max entry is 5120 B, max payload is 4068 B
```

- `adb logcat -G 5M -b system`
- `adb logcat -G 5M -b main`
- `adb logcat -G 15M -b main`
- `adb logcat -G 5M -b main`


# APK install
- `adb shell install <apk>` - Install app available on local disk
- `adb shell install <path>` - Install app from phone path
- `adb shell install -r <path>` - Install app from phone path
- `adb shell uninstall <name>` - Remove the app

# Flashing
- `adb reboot download` - Reboot the device in reflashing mode (Odin)
- `adb reboot bootloader` - Reboot the device in reflashing mode (Odin)
- `adb reboot fastboot` - Reboot the device into fastboot  mode
- `adb reboot recovery` - Reboot the device into recovery mode

# Record/Capture
- `adb shell screencap -p "/path/to/screenshot.png"` - capture screenshot
- `adb shell screenrecord "/path/to/record.mp4"` - record device screen
   - `adb shell screenrecord --time-limit 180 /data/local/tmp/test.mp4`

# Adb shell top
- `adb shell top -H` - Displays the threads
- `adb shell top -m numberOfTasks` - Displays the specified number of tasks
- `adb shell top -H` - Show FIELDS (`def PID,USER,PR,NI,VIRT,RES,SHR,S,%CPU,%MEM,TIME+,CMDLINE`)
- `adb shell top -o %CPU,%MEM,TIME+` - Maximum number of tasks to show

# Permissions
- `adb shell permissions groups` - list permission groups definitions
- `adb shell list permissions -g -r` - list permissions details

# Package Info
- `adb shell pm list packages` - list package names
- `adb shell pm path <package-name>` - Get path of the apk file

# ndk-build
- `ndk-build compile_commands.json` - Generate compile commands json file
- `ndk-build NDK_DEBUG=1` - Compile code with debug info
- `ndk-build V=1` - Compile code and show build command(s)
- `ndk-build -B` - Force rebuild

# Misc
-  `aapt dump badging Khronos-CTS.apk | grep -i "launchable-activity"` - Get activity name
- To find launch-able activity nameo of the applicaiton
    - Launch app
    - Use `dumpsys window displays | grep -E "mCurrentFocus"`
- View memory info
    ```bash
    adb shell pidof com.hello.world
    adb shell dumpsys meminfo package_name|pid [-d]
    adb shell dumpsys meminfo com.android.settings | grep -w 'Native Heap[ ]'
    ```

# Switching to usb mode for adb
- Setup USB mode for adb to avoid popup message on connecting usb to pc
```bash
adb shell setprop persist.sys.usb.config adb
adb shell setprop sys.usb.config adb
```

## To find touch events
```bash
adb shell getevent -l
ABS_MT_POSITION_X
```

## Sending input events from adb
```bash
adb shell input swipe 300 300 500 1000
adb shell input swipe 500 1000 300 300
```

```bash
adb shell input keyevent 82
adb shell input keyevent KEYCODE_APP_SWITCH
adb shell input keyevent KEYCODE_APP_SWITCHER
adb shell input keyevent KEYCODE_CLOSE
adb shell input keyevent KEYCODE_DPAD_DOWN
adb shell input keyevent KEYCODE_ENTER
```
