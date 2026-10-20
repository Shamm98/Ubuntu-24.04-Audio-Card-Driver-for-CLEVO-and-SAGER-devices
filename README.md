# Ubuntu-24.04-Audio-Card-Driver-for-CLEVO-and-SAGER-devices
Fix on Audio driver for new SAGER and CLEVO devices not functioning on UBUNTU 24.04 (working and tested 👍🏾)

This issue is a fix for the audio codec driver Realtek ALC1220 chip, which is the built-in Audio Analog Stereo for my Sager 2023 NP8875E Gaming Laptop.

Ubuntu 24.04 LTS has known audio problems on laptops, primarily because it switched the default audio server from PulseAudio to PipeWire. Common Audio Issues include:

• Crackling and Popping (Xruns): Conflicting apps still trying to use legacy PulseAudio or mismatched buffer sizes cause audio dropouts in apps like Zoom, Steam, and video players.
• Missing Speakers / "Dummy Output": Internal laptop speakers fail to be detected or show up only as "Dummy Output," while headphones or Bluetooth audio may still work.
• Flat or Distorted Sound: Some users report built-in laptop speakers sounding muffled, distant, or quiet compared to Windows or older Ubuntu versions.
• Application-Specific Dropouts: Audio cutting out or crashing specifically inside conferencing or VOIP tools (such as Zoom or Jabra USB devices).

The CLEVO system uses PipeWire instead of PulseAudio (the default on current Ubuntu, Fedora, Debian and others). But as a bonus PipeWire provides a PulseAudio-compatible layer.

Whenever there are issues on this layer, this is how users typically troubleshoot 
	
• Restart the Audio Stack: Restart PipeWire and WirePlumber services in the terminal:bash
	
	systemctl --user restart pipewire pipewire-pulse wireplumber

• Install Volume Control: Use GUI tools like pavucontrol to re-route audio streams or fix default device selection:bash

    sudo apt install pavucontrol

and check whether the driver is working using:	
	
	wpctl status

I have tried these and many other proposed solutions, only to be disappointed with the outcomes. Even when I tried to check the status of the audio options using the command

	alsamixer -c 0

everything was healthy and working fine. And one of the most annoying parts is when one checks out every possible solutions, makes sure everything is working and tests the hardware, only to find out that there is no output coming. That is what happened when I tested my PC using the command

	speaker-test -c 2 -t wav -D hw:0,0

and still no sound came out.


But I continued to the Clevo systems' previous working drivers and documentations and was lucky enough to find out that the newer generation of Sager PCs followed similar mother board design and that the driver that works for the older models works for the new ones as well. But note that this is assuming the devices designed after the Sager NP8875E follow similar designs. This is the closest existing solution for the ALC1220 Clevo fix up as there are no officially existing solutions.

All you have to do for the driver to run on your device is use a Clevo Quirk:

	echo "options snd-hda-intel model=clevo-p950" | sudo tee /etc/modprobe.d/alsa-clevo.conf

which puts the problem at the speaker amp not being switched on by the generic driver, where the Clevo quirk handles it.
