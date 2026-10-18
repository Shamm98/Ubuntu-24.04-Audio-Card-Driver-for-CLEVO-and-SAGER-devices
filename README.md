# Ubuntu-24.04-Audio-Card-Driver-for-CLEVO-and-SAGER-devices
Fix on Audio driver for new SAGER and CLEVO devices not functioning on UBUNTU 24 (working and tested 👍🏾)

This issue is a fix for the audio codec driver Realtek ALC1220 chip, which is the built-in Audio Analog Stereo for my Sager 2023 NP8875E Gaming Laptop.

The CLEVO system uses PipeWire instead of PulseAudio (the default on current Ubuntu, Fedora, Debian and others). But as a bonus PipeWire provides a PulseAudio-compatible layer, which makes this part easier.
