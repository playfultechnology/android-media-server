# android-media-server
Notes on hacking various Android-based media devices

Theatres, immersive experiences, and escape rooms are increasingly making use of video effects displayed on monitors and screens or projected onto surfaces. 
These would ideally be controlled via centralised show control software which could either deliver media over a network, or at least send a signal to trigger playback of a local media file.

# Hardware Choices

| Device | Cost | Notes |
| --- | --- | --- |
| Sprite | ![150](https://img.shields.io/badge/£150-orange) | Can be paired with ESP32/Arduino to trigger network playback |
| Raspberry Pi | ![150](https://img.shields.io/badge/£150-orange) | Requires RPi4/5 for reliable 1080p playback |
| NUC PC + VLC etc. | ![200](https://img.shields.io/badge/£200-red) | Most powerful, possible dual HDMI output, but also most expensive |
| Android-based "HD Media Player" | ![25](https://img.shields.io/badge/£25-green) | Cheapest |

There are many "Media Players" available on AliExpress etc. which are running some variant of the Android operating system and would seem to have all the hardware required - they have (normally wireless, sometimes wired) network connection, HDMI output, can access media on USB storage. The problem is that they are not designed to be controlled by anything other than their supplied IR remote.
One alternative then is to use an IR blaster device to clone and replay the command signals sent by the controller.
Another is to try to modify and control the built-in Android operating system to remotely trigger playback of files from the USB/SD storage.

## Amazon Fire Stick

Note that Amazon Fire sticks etc. do not generally have any local storage, nor the ability to load files from USB storage.

 - External USB support _can_ be added using https://www.amazon.co.uk/Power-Cable-Firestick-Lite-Cube-Black/dp/B07XTSQ8ZY
 - Wired ethernet support _can_ be added using https://www.amazon.co.uk/Ethernet-Adapter-Compatible-Chromecast-Streaming-1PCS/dp/B0GR8WM124

## Wimius P62 Projector
This is running a cut-down version of Android that has ADB access already authorised, which means it's possible to connect to and control over a network using ADB. 
Set up the projector to access the local Wi-Fi, and take a note of the assigned IP address.
Then issue ADB commands from a PC on the same network. 

### 1.) Download Android developer tools
https://dl.google.com/android/repository/platform-tools-latest-windows.zip

### 2.) Connect to the projector's IP address
`adb connect 192.168.1.42:5555`  (e.g.)

Check it is connected
`adb devices`

### 3.) List available apps
`adb shell pm list packages`

Check available space
`adb shell df /`

### 4.) Upload an image file
`adb push black_1920x1080.png /sdcard/black_1920x1080.png`

### 5.) Display an image file using default viewer
`adb shell am start -a android.intent.action.VIEW -d file:///sdcard/black_1920x1080.png -t image/png`

### 6.) Delete the file
`adb shell rm /sdcard/black_1920x1080.png`

### 7.) List all .MP4 files on the USB drive
`adb shell find /storage/sda1 -iname "*.mp4"`

### 7b.) List all files on the SD card
`adb shell ls -l /sdcard/`

### 8.) Play a video file using specific viewer
`adb shell am start -a android.intent.action.VIEW -p com.hisilicon.android.videoplayer -d "file:///storage/sda1/Airplane.mp4" -t video/mp4`
or
`adb shell am start -a android.intent.action.VIEW -n com.newlink.cast/tv.danmaku.ijk.media.example.activities.VideoActivity -d "file:///storage/sda1/Pirate.mp4" -t video/mp4`

### 9.) List all events
`adb shell getevent -lp`

### 10.) Issue command
`adb shell input keyevent KEYCODE_MEDIA_NEXT`

or by number:
`adb shell input keyevent 87`

List of events

KEYCODE_HOME                3      Home
KEYCODE_BACK                4      Back / exit
KEYCODE_MENU                82     Menu / player options
KEYCODE_SETTINGS            176    Settings
KEYCODE_DPAD_UP             19     Up
KEYCODE_DPAD_DOWN           20     Down
KEYCODE_DPAD_LEFT           21     Left / rewind in some players
KEYCODE_DPAD_RIGHT          22     Right / forward in some players
KEYCODE_DPAD_CENTER         23     OK / select
KEYCODE_ENTER               66     Enter / select
KEYCODE_ESCAPE              111    Escape / back-like

KEYCODE_MEDIA_PLAY_PAUSE    85     Play/pause toggle
KEYCODE_MEDIA_STOP          86     Stop
KEYCODE_MEDIA_NEXT          87     Next item
KEYCODE_MEDIA_PREVIOUS      88     Previous item
KEYCODE_MEDIA_REWIND        89     Rewind
KEYCODE_MEDIA_FAST_FORWARD  90     Fast-forward
KEYCODE_MEDIA_PLAY          126    Play
KEYCODE_MEDIA_PAUSE         127    Pause
KEYCODE_MEDIA_CLOSE         128    Close media
KEYCODE_MEDIA_EJECT         129    Eject
KEYCODE_MEDIA_RECORD        130    Record
KEYCODE_MEDIA_SKIP_FORWARD  272    Skip forward
KEYCODE_MEDIA_SKIP_BACKWARD 273    Skip backward
KEYCODE_MEDIA_STEP_FORWARD  274    Step forward
KEYCODE_MEDIA_STEP_BACKWARD 275    Step backward

KEYCODE_POWER               26     Power toggle; you found this turns projector off
KEYCODE_SLEEP               223    Sleep; did nothing on your firmware
KEYCODE_WAKEUP              224    Wake; only works if ADB remains alive
KEYCODE_TV_POWER            177    TV/projector power
KEYCODE_TV_INPUT            178    Input/source
KEYCODE_STB_POWER           179    Set-top-box power
KEYCODE_STB_INPUT           180    Set-top-box input
KEYCODE_AVR_POWER           181    AV receiver power
KEYCODE_AVR_INPUT           182    AV receiver input
KEYCODE_GUIDE               172    Guide
KEYCODE_CAPTIONS            175    Captions/subtitles
KEYCODE_INFO                165    Info / OSD
KEYCODE_TV                  170    TV
KEYCODE_WINDOW              171    Window / PiP-like on some builds
KEYCODE_CHANNEL_UP          166    Channel up / sometimes next
KEYCODE_CHANNEL_DOWN        167    Channel down / sometimes previous
KEYCODE_TV_MEDIA_CONTEXT_MENU 257  TV media menu

KEYCODE_VOLUME_UP           24     Volume up
KEYCODE_VOLUME_DOWN         25     Volume down
KEYCODE_VOLUME_MUTE         164    Mute
KEYCODE_MUTE                91     Mute mic/audio route on some builds

KEYCODE_PROG_RED            183
KEYCODE_PROG_GREEN          184
KEYCODE_PROG_YELLOW         185
KEYCODE_PROG_BLUE           186

KEYCODE_0                   7
KEYCODE_1                   8
KEYCODE_2                   9
KEYCODE_3                   10
KEYCODE_4                   11
KEYCODE_5                   12
KEYCODE_6                   13
KEYCODE_7                   14
KEYCODE_8                   15
KEYCODE_9                   16
KEYCODE_STAR                17
KEYCODE_POUND               18

KEYCODE_APP_SWITCH          187    Recent apps
KEYCODE_SEARCH              84     Search
KEYCODE_ASSIST              219    Assistant
KEYCODE_BRIGHTNESS_DOWN     220    Brightness down
KEYCODE_BRIGHTNESS_UP       221    Brightness up
KEYCODE_NOTIFICATION        83     Notification panel
KEYCODE_SOFT_SLEEP          276    Soft sleep, if supported



