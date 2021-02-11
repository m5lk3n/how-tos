# How to install LineageOS 17.1 with Open GApps 10.0 on a Pixel 2 XL via USB Stick

*This description has yet to be double-checked though the underlying technical solution has been verified several times :-\ !*

**WARNING:**

- YOU MAY BRICK YOUR DEVICE!
- ALL DATA WILL BE WIPED FROM YOUR DEVICE!
- USE WITH CAUTION AND AT YOUR OWN RISK!
- I'M NOT RESPONSIBLE FOR YOUR BRICKED DEVICE OR YOUR LOST DATA!
- YOU ARE ON YOUR OWN WITH THIS - NO SUPPORT, NO GUARANTEE!
- APPLIES ESPECIALLY TO PIXEL 2 XL AND LINEAGE OS 17.1 ONLY!

This is an installation protocol that might be useful for other, tech-savvy people who are a bit familiar with Linux, Android and are comfortable flashing a device. To be honest, I struggled to either understand/apply descriptions (like [this one](https://wiki.lineageos.org/devices/taimen/install) - no offense, it's me) or to find concise online material which solved my specific case (as outlined in "The Situation" section next). Therefore, I compiled this document.

## The Situation

Unfortunately, my Pixel 2 XL had reached end of Android support at the end of 2020 which meant not even security patches anymore :-( Yet, the phone as such was still in a pretty good shape including a long-lasting battery. So, no reason at all to get rid of this excellent device.

So, I had a Pixel 2 XL

- without a SIM card (which had moved to my new Pixel 5)
- in "Developer mode" (see steps 1-3 [here](https://wiki.lineageos.org/adb_fastboot_guide.html#setting-up-adb))
- with "USB debugging" enabled under "Developer options" (see step 4 [here](https://wiki.lineageos.org/adb_fastboot_guide.html#setting-up-adb))
- *with a greyed out option for "OEM unlocking" the bootloader* in the same "Developer options" menu

Especially the latter turned out to be tricky...

My goal was to install the latest, stable LineageOS along with some Google Apps to keep the old Pixel alive for my family (or as a spare option).

Also noteworthy, neither `adb sideload` nor `adb push` worked in my case to transfer binaries to the device. Many people suggested to use a different USB cable in such a case of stopped or no transfers; I tried four other cables to no avail however.

Hence, my idea via USB stick. Here comes the solution...

## The Requirements

### Binaries

I used the following downloads:

- [TWRP for "taimen"](https://dl.twrp.me/taimen/) - the temporary tool required to flash LineageOS and Open GApps onto the phone, more specifically:
  - [this img file](https://dl.twrp.me/taimen/twrp-3.5.0_9-0-taimen.img.html), to be flashed from Linux onto the device
  - [this zip file](https://dl.twrp.me/taimen/twrp-installer-3.5.0_9-0-taimen.zip.html), to be flashed from the device onto the device
- [LineageOS build for "taimen"](https://download.lineageos.org/taimen), to be more precise [this 17.1 build](https://mirrorbits.lineageos.org/full/taimen/20210107/lineage-17.1-20210107-nightly-taimen-signed.zip) (there are no release builds, only nightlies).
- [Open GApps](https://opengapps.org/) - Google applications missing in LineageOS. To download the package, select "ARM64" as platform, "10.0" as the Android version matching LineageOS 17.1, and "micro" as variant (or any variant smaller than that; "aroma" left my device in a recovery bootloop and other packages turned out to be simply too big for the storage remaining on my phone). In my case, it turned out to be [this specific package](https://sourceforge.net/projects/opengapps/files/arm64/20210114/open_gapps-arm64-10.0-micro-20210114.zip/download)).

### Hardware

Below are my prerequisites in terms of hardware.

#### Accessoires

- Yet another USB stick with at least 1,1 GB of free space (I had mine ext4 formatted)
- A USB-(male)-to-USB-C cable (any will do!) to connect the phone to the computer
- A USB-C-to-USB-(female) adapter to plug in the USB stick and to connect to the phone later (to transfer LineageOS, GApps, and TWRP)

#### Computer

I work on yet another, Ubuntu-based Linux.

Install "adb" and "fastboot" as per [this description](https://wiki.lineageos.org/adb_fastboot_guide.html). Hint: I did *not* follow [this link from the document](https://github.com/M0Rf30/android-udev-rules#installation), but followed [the official Android documentation](https://developer.android.com/studio/run/device) instead, i.e.:

```bash
$ sudo usermod -aG plugdev $LOGNAME
$ apt-get install android-sdk-platform-tools-common
# I had to reboot my machine at this point to make proper use of adb. There is probably a better way to just restart certain services which I didn't bother to figure out. Sorry, I could have done better here. 
```

#### Phone

Make sure to have 60%+ battery charge (as people say, not sure where this figure is coming from but it won't hurt to safely flash the device).

## The Solution

First, boot the phone into recovery mode:

- With "Android debugging" enabled under the "Developer options" on the phone, leave the phone turned on, connect it via USB cable to the computer, allow connections from the computer on the Pixel, then type the following:

  ```bash
  # just to make sure your computer is properly aware of the phone:
  $ adb devices
  # I found the following option to boot into recovery mode easier than finding and holding the right button press combination while starting the turned-off device itself:
  $ adb reboot recovery
  ```

- Once the device has restarted, choose "Recovery mode" which, in my case, eventually ended up with the "No command" screen.

- On this screen, holding the power button, press the "volume up" button shortly and release it again while still keeping the power button pressed. This brought me to the Android recovery screen.

- From the recovery options screen, I chose all wipe options and restarted the device. *That was the key prerequisite to enable the greyed out "OEM unlock" option in the "Developer options" again!*

- On the phone, go back to "Developer options" and enable "OEM unlock", then `adb reboot recovery` from the computer again.

Second, flash the TWRP utility ROM from the computer to the Pixel as follows:

```bash
$ fastboot flash boot twrp-3.5.0_9-0-taimen.img
Sending 'boot_a' (40960 KB)                        OKAY [  1.205s]
Writing 'boot_a'                                   OKAY [  0.373s]
Finished. Total time: 1.582s
```

Choose "Start" on the phone screen which will boot into TWRP utility.

Third, prepare the device for installation via USB flash drive, not via USB cable:

- Unplug the phone's USB connection to the computer.
- Unlock TWRP, go to "Reboot", make sure to have "Slot B" selected as the active partition (was the default in my case). Do not reboot, but go back to the main menu, press "Wipe" and "Format Data".
- Go back to "Reboot" and select "Slot A". Again, no need to restart. Go back to format this partition the same way as before. This is just to make sure that all partitions are properly wiped.
- Connect the USB stick via USB-to-USB-C adapter to the phone.
- In TWRP's main menu, go to "Advanced", tap "Terminal" to mount the flash drive as follows:

  ```bash
  $ mkdir /mnt/usb
  $ mount /dev/block/sdh1 /mnt/usb
  # in my case /dev/block/sdh1 was where the flash drive showed up in the majority of the cases (sdi1 in 1 case), use 'dmesg | grep -i flash' and 'dmesg | grep EXT4' for potential trouble-shooting
  ```

Fourth, flash the Lineage zip and the TWRP zip file onto the device (TWRP is needed to flash the GApps later):

- On TWRP, just make sure to remain on "Slot A" (see "Reboot" menu), go back to the main menu, and tap "Install", then "Storage". Select "Storage 1" to access the USB stick with the zip files.
- Install the lineage image by clicking on the zip file. This will install LineageOS on partition B! (The Pixel is a so-called [A/B device](https://source.android.com/devices/tech/ota/ab) with two separate partitions for seamless updates; it's less likely to remain in an unusable state after an update.) Make sure to *not* have the reboot option selected as we need to continue the installation. Confirm flash and wait some minutes. Again, do *not* reboot as it'll ultimately boot into LineageOS which replaces the custom TWRP bootloader at this time!
- Go back to the main menu, under "Reboot", switch to "Slot B". This is just to make sure that the next zip file (the TWRP one) will be flashed to the proper boot partition. Though this step might not be necessary as this recent TWRP installer seems to install into both partitions, A and B, it initially made sense to me to ensure installation of the utility into the right partition.
- This step is absolutely crucial to enable the next step, installation of GApps: Head back to the main menu, under "Install", select the TWRP zip file from the USB flash drive, and "Reboot" to "Recovery" afterwards. This will boot the device into TWRP again, not into LineageOS at this point in time.

Fifth, install the GApps like so:

- In TWRP, make sure under "Reboot" to have "Slot B" active.
- Back in the main menu, follow "Advanced" and tap "Terminal". Mount the USB flash drive again: `mkdir /mnt/usb && mount /dev/block/sdh1 /mnt/usb`
- Go back to the main menu, choose "Install", then click the GApps zip to install those apps into the existing LineageOS partition. Once completed, finish the installer by "Reboot" into "System". Unplug the USB adapter during reboot. Initial LineageOS startup takes a few moments, but don't worry.

Continue to enjoy your Pixel 2 XL on LineageOS!

## Miscellaneous

### Useful Bookmarks

- [HardReset.info: Recovery Mode GOOGLE Pixel XL 2](https://www.hardreset.info/devices/google/google-pixel-xl-2/recovery-mode/)
- [OpenGApps daily releases](https://sourceforge.net/projects/opengapps/files/arm64/)

### Open GApps "aroma" flavor

I tried this variant following the customized installation path to choose the Google apps that I want - to find out which apps to leave out, use TWRP's built-in terminal to do e.g. `more open_gapps_log.txt` which gives you a list with app sizes to look at. For some reason, installing this flavor ended up in aforementioned bootloop. There's a chance to avoid that by choosing "Wipe Dalvik" prior to the final "Reboot" into "System". As I was able to finish the installation based upon the "micro" flavor, I was able to install remaining Google apps via the installed Play Store.

### Potential Outlook

- [Install Android 11 on Pixel 2 & 2 XL (LineageOS 18) – How to Guide!](https://theupgradeguide.com/install-android-11-on-pixel-2-2-xl-lineageos-18-how-to-guide/)
- [NikGApps](https://forum.xda-developers.com/t/arm64-gapps-android-11-nikgapps-31st-dec-2020.3915866/)
