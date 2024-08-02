# Android 13,14 and 15 GSI on RMX2193/2191/3171 : Installations and Fixes

> Android 15 GSIs are not mature enough yet and Android 14 GSIs have some issues with IMS and Bluetooth. So I recommend you use Android 13 GSIs for now.


<summary>Table of contents</summary>
<ol>
<li><a href="#part-0--precautionsrequirementsdisclaimers">Part 0 : Precautions/Requirements/Disclaimers</a></li>
<li><a href="#part-1--installing-base-firmware-for-the-gsi">Part 1 : Installing Base Firmware for the GSI</a></li>
<li><a href="#part-2--installing-the-gsi-commands-assume-youre-using-a13-pixel-experience-plus-change-accordingly-if-youre-using-a-different-gsi">Part 2 : Installing the GSI</a></li>
<li><a href="#part-3--post-installation-fixes">Part 3 : Post Installation Fixes</a></li>
<li><a href="#part-4--banking-apps">Part 4 : Banking Apps</a></li>
<li><a href="#credits">Credits</a></li>
</ol>



## Part 0 : Precautions/Requirements/Disclaimers

⚠️ Before you proceed, these things first : 

1. Visit [the community](/GSI/rmx_2193_community.md) for any troubleshooting and discussions, not me. I am merely cumulating information in one place for your convenience. If I am spammed for support, I will just take this guide down and refuse to maintain it further.

2. You can find community archives of some device related files [here](https://t.me/rmx3171important)

3. No one owes you anything. If you are stuck, be patient and ask for help in the community. If they can't help, you are on your own.

4. These guides assume you have 

- A working brain, computer, internet connection, and a USB cable.
- An SD card with at least 8 GB of space or a USB OTG drive. You could also get away with the internal storage, but it's a bit complicated based on what you're trying to do and if the storage is encrypted. Use common sense in case of internal storage.
- A bootloader unlocked RMX2193/2191/3171 device with a custom recovery installed on realme UI 2 base firmware. If you're not there yet, visit [the community](/GSI/rmx_2193_community.md) for guides. I might add them here later.

- [platform-tools](https://developer.android.com/tools/releases/platform-tools) installed on your computer. I am assuming you know how to use ADB and Fastboot. If not, learn.

4. Everything linked here might not be fully open source and may not be properly audited for security. It doesn't mean they are malicious, I am just dropping off my personal liability here. For instance I myself use the exact firmwares, GSI and tools linked here on my personal device, so you can be a bit assured.

5. Known bugs that has no fixes yet : 

    1. Reboot functionality is broken. You can however shut down and power on the device normally. Attempting to reboot will also shut down the device, so you'll have to power it on manually.

    2. Wi-Fi calling is broken and can't be fixed. (Wi-Fi calling doesn't mean VoIP calls like WhatsApp, it's a carrier feature that allows you to make calls over Wi-Fi when you have no cellular signal)

    3. System screen recorder is broken, use a third party app.

## Part 1 : installing base firmware for the GSI

> This ensures your partitions are in the right state for modern GSIs (>=A12) to boot.  

1. Download base firmware file from [here](https://www.mediafire.com/file/z4c9mvr2z4uz854/gsi_base_firmware.zip/file) and A13 Pixel Experience Plus from [here](https://github.com/ChonDoit/treble_peplus_patches/releases/download/A13/PE-Plus_A13-RO-arm64-bgN-slim_20240629.img.xz).   
You can theoretically use any GSI (>=A11) that has RO /system partition, stay away from vndlite GSIs (I haven't figured out how to boot them yet) and official GSIs directly from Google. 
Here is a list of [GSIs](https://github.com/phhusson/treble_experimentations/wiki/Generic-System-Image-%28GSI%29-list).   
This guide is tested with [A13 Pixel Experience Plus](https://github.com/ChonDoit/treble_peplus_patches/releases/download/A13/PE-Plus_A13-RO-arm64-bgN-slim_20240629.img.xz), so I recommend you use that if you're relatively new to this.

2. Use common sense and save the file to your SD card or USB OTG drive or (if you know what you're doing) internal storage.
3. Assuming you have a custom recovery installed on a realme UI 2 base firmware, boot into it. 
4. Flash the base firmware zip file you downloaded in step 1.
5. Reboot to bootloader. 

## Part 2 : Installing the GSI (commands assume you're using A13 Pixel Experience Plus. Change accordingly if you're using a different GSI)

1. Depending on your operating system, get a relevant tool to extract the xz file. You should have `PE-Plus_A13-RO-arm64-bgN-slim_20240629.img` file after extraction.
2. Move the .img file to your platform-tools folder. 
3. Download [vbmeta.img](https://github.com/realKarthikNair/phone-configs/raw/master/GSI/vbmeta.img) and [boot.img](https://github.com/realKarthikNair/phone-configs/raw/master/GSI/boot.img) move them to your platform-tools folder.
4. Open a command prompt or terminal in the platform-tools folder. Assuming your device is in bootloader mode from Part 1 of this guide, run the following commands : 

    ```bash
    fastboot reboot fastboot
    fastboot erase system
    fastboot flash system PE-Plus_A13-RO-arm64-bgN-slim_20240629.img
    fastboot -w
    fastboot flash boot boot.img
    fastboot flash vbmeta --disable-verity --disable-verification vbmeta.img
    fastboot reboot recovery
    ```
5. Once you're in recovery, 

    1. Flash [this kernel](https://github.com/realKarthikNair/phone-configs/raw/master/GSI/neOlit+kernel.zip)
    2. Don't reboot, but go back
    3. Format data
    4. Reboot to system
    5. If you don't see the realme logo in 2-3 seconds, hold the power button to boot. 

6. The first boot will take a while, so be patient. If you face any issues, visit [the community](/GSI/rmx_2193_community.md) for help.

## Part 3 : Post installation fixes

> Your phone calls, SMS, hotspot and bluetooth might not work right after boot, but please be patient and follow these steps to fix them.

1. Once you're in the system, go to settings and enable developer options by tapping on the build number 7 times.
2. Go to developer options and enable USB debugging.
3. Download [magisk apk file](https://github.com/realKarthikNair/phone-configs/raw/master/GSI/magisk.apk) and move to your platform-tools folder.
4. Download [magisk_patched.img](https://github.com/realKarthikNair/phone-configs/raw/master/GSI/magisk_patched.img) and move to your platform-tools folder.
5. Open a command prompt or terminal in the platform-tools folder and run the following commands : 

    ```bash
    adb install magisk.apk
    adb reboot bootloader
    fastboot flash boot magisk_patched.img
    fastboot flash vbmeta --disable-verity --disable-verification vbmeta.img
    fastboot reboot
    ```

6. Once you're in the system, open magisk and follow the instructions to install it and reboot.
7. Once you're in the system, open magisk settings and 

    1. Turn Off "check for updates"
    2. Turn on "Zygisk" and "Enforce Denylist"
    3. Go to "Configure Denylist" and from the top right menu, check "Show system apps" and "Show OS apps".
    4. Add these apps : 

        - Google Play Services
        - Google Play Store
        - Google Services Framework

    5. Go back to magisk app home. 

8. From bottom bar of magisk app, Go to modules and install modules in [modules folder](/GSI/modules) one by one. Dont reboot at all now (press cancel when it asks to reboot). I haven't made those modules, so for any concerns, visit [the community](/GSI/rmx_2193_community.md). **These modules fix Hotspot, Bluetooth, Safetynet and Play Integrity.**. Don't reboot yet (follow the next steps now).

9. Go to phone settings > Phh Treble settings 
    

    1. If you want Double tap to wake, go to Oppo features > Enable DT2W
    2. To fix bluetooth
        - Go to Misc features > AUDIO > Use System Wide BT HAL, and
        - Go to Misc features > AUDIO > Bluetooth workarounds > Mediatek
    3. To fix 3.5 mm audio jack
        - Go to Misc features > AUDIO > Use alternate way to detect headsets
    4. To fix status bar top-right padding
        - Go to Misc features > USER INTERFACE > Set Status bar end padding to 0
    5. To fix brightness slider glitches, scale and thresholds
        - Go to Misc features > BACKLIGHT > Set alternative brightness curve
        - Go to Misc features > BACKLIGHT > Force alternative brightness scale
        - Go to Misc features > BACKLIGHT > Allows setting brightness to the lowest possible
    6. To fix auxillary cameras
        - Go to Misc features > CAMERA > Expose Aux Cameras
        - Go to Misc features > CAMERA > Force Enable Camera2API HAL3
    7. To fix VoLTE
        1. Go to IMS features
        2. Tap on "Create IMS APN"
        3. Tap on "Install IMS APK for MediaTek R Vendor" and install the apk.
        4. toggle "Request IMS network" and "Force the presence of 4G Calling setting" ON.

10. Go to settings > Apps > See all apps > top right corner > Show system apps > clear data and cache of Google Play Services, Google Play Store and Google Services Framework. Reboot now. 

11. You should have a phone with everything working now. If you face any issues, visit [the community](/GSI/rmx_2193_community.md) for help.

## Part 4 : Banking apps 

> All of them won't work. But PayTM and SBI Yono Lite worked for me. Here's how. I am assuming you have installed the PlayIntegrityFix module from Part 3 of this guide.

1. Install PayTM and SBI Yono Lite from Play Store. Don't open them yet.
2. Open magisk settings and Go to configure denylist and add PayTM and SBI Yono Lite.
3. Reboot.

## Credits 

- [Sarthak Roy](https://t.me/sarthakroy2002) for recoveries, kernels, trees and patches.
- [ĶѦℛѦひﾚÏ ᏦÏᏁᏳ](https://t.me/Karauli01king) for making GSI ROM flashable via recovery (builds available in [the community](/GSI/rmx_2193_community.md)).
- [L Lawliet](https://t.me/l_6174) for OFRP, SHRP and TWRP recoveries.
- Testers : [https://t.me/CYBER6756, https://t.me/usernameadittya, https://t.me/rockstar_jp]
- [phhusson](https://github.com/phhusson) and [TrebleDroid Team](https://github.com/TrebleDroid) for making Android GSIs possible on non-Pixel devices with ease. 
- [ChonDoit](https://github.com/ChonDoit) for making A13 Pixel Experience Plus GSIs.
- [The Magisk Project](https://github.com/topjohnwu/Magisk)
- [chiteroman](https://github.com/chiteroman) for PlayIntegrityFix
- Admins of [RMX2193/2191/3171 Community](/GSI/rmx_2193_community.md).
- [Me](https://github.com/realKarthikNair) for writing this guide and finding fixes to some issues.
- Anyone I missed, please let me know. 
