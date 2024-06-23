# NoLatencyLegionAudio
The Legion Pro 7 has awful latency on Windows when using the drivers and APOs provided by default, typically 90ms - 120ms latency on speaker output.
This .reg file is a successor to NoNahimic but specifically for the Legion Pro 7.

This repo provides a .reg file that blocks Nahimic/A-Volute and Elevoc APOs and drivers from installing. This does NOT block the Realtek Driver, which is required to have working audio on a fresh boot due to the TI amps.
Currently its not known how to block the `Realtek Semiconductor Corp. - Extension 10.35.220.64` - which is the main cause of the issue.
As such you will have to periodically uninstall this Extension (not the driver) with RAPR, and Pause Windows Updates to prevent it from installing.

## Before and After videos:
See before.mp4 in the repo
See after.mp4 in the repo

## How do I check my current output latency?
1. Play the 'Sync-Footage-V1-H264.mp4' file 
2. Use an external capture device, such as a smart phone. Ideally capture in high frame rate mode, or in the case of Android there is a "Slow Motion" option that can be set to 1/8th speed.
3. Capture the screen, then view the recording on your phone. It should be apparent if you are not having 0ms latency.

## Confirmed Devices
* Lenovo Pro 7i 16IRX8H (Gen 8, 2023)

## Currently Blocked Hardware IDs
* ROOT\Nahimic_Mirroring -- Nahimic/A-Volute
* SWC\VEN_AVOL&AID_0300 -- Nahimic/A-Volute
* SWC\VEN_AVOL&AID_0400 -- Nahimic/A-Volute
* ROOT\NahimicBTLink -- Nahimic/A-Volute
* ROOT\NahimicXVAD -- Nahimic/A-Volute
* SWC\VEN_AVOL&AID_0802 -- Nahimic/A-Volute
* SWC\VEN_ELEVOC&AID_0001 -- Elevoc
* SWC\RGB -- Tobii Eye Tracking (Not related, but has a misleading HWID, and ultimately is bloated software. Remove this line in the reg if you want to use Tobii)

## How to Get 0-latency audio output on your built in speakers
Unfortunately, the .reg block must be run/done BEFORE connecting to the internet in a fresh Windows installation (last tested Windows 11, 23H2).
If Nahimic/Elevoc drivers are installed at any point, configurations will cause problems and no speaker output will work, or latency will be present.

0. Note: The touchpad driver will not be installed until Internet is connected. You must use an external mouse or navigate with keyboard only until then.
1. Fresh Install Windows 11 WITHOUT connecting to the Internet. You can use Ventoy with a Windows 11 ISO, which will automatically remove the Internet requirement without need for OOBE\BYPASSNRO
2. Run the .reg file from this repo, and reboot.
3. Connect to the Internet, let Windows Update fully, and install whatever drivers. You may see errors from the blocks from the reg, this is okay to ignore.
4. Note: NVIDIA gpu may not be detected. To resolve this, download LTT: https://github.com/BartoszCichecki/LenovoLegionToolkit and change "Hybrid-GPU" to "Hybrid". Reboot. Then let Windows Update install a stock nvidia driver. From here, you can install the latest nvidia drivers as usual after.
5. Once your system is fully up-to-date, perform the "How do I check my current output latency" steps. You will notice there is still latency.
6. Download RAPR (DriverStoreExplorer): https://github.com/lostindark/DriverStoreExplorer and "Force Delete/Force Remove" under the "Extensions" section: `Realtek Semiconductor Corp. - Extension 10.35.220.64`
7. Reboot, then perform the "How do I check my current output latency" steps. You should have 0 latency.
8. Unfortunately, currently I do not know a method to block Extension from installing, so every Windows Update your audio will revert to high latency. I recommend blocking for 7 days once you do a check and remove the Extension. Repeat steps 6-7 as needed.
9. Important, do not ever install audio drivers manually. Other drivers are safe to install.

