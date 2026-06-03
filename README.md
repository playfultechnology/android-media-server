# android-media-server
Notes on hacking various Android-based media devices

Theatres, immersive experiences, and escape rooms are increasingly making use of video effects displayed on monitors and screens or projected onto surfaces. 
These would ideally be controlled via centralised show control software which could either deliver media over a network, or at least send a signal to trigger playback of a local media file.

# Hardware Choices

| Device | Cost | Notes |
| Sprite | ![150](https://img.shields.io/badge/150-amber) | Can be paired with ESP32/Arduino to trigger network playback |
| Raspberry Pi | ![150](https://img.shields.io/badge/150-amber) | Requires RPi4/5 for reliable 1080p playback |
| NUC PC + VLC etc. | ![200](https://img.shields.io/badge/200-red) | Most powerful, possible dual HDMI output, but also most expensive |
| Android-based "HD Media Player" | ![25](https://img.shields.io/badge/25-green) | Cheapest |

There are many "Media Players" available on AliExpress etc. which are running some variant of the Android operating system and would seem to have all the hardware required - they have (normally wireless, sometimes wired) network connection, HDMI output, can access media on USB storage. The problem is that they are not designed to be controlled by anything other than their supplied IR remote.
One alternative then is to use an IR blaster device to clone and replay the command signals sent by the controller.
Another is to try to modify and control the built-in Android operating system to remotely trigger playback of files from the USB/SD storage.







