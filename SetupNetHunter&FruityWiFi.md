# Install Kali Nethunter & FruityWiFi on Nexus 5
Originally Written by Ben May
Twitter: https://twitter.com/atarii
Git: https://github.com/atarii

Here are the steps taken to install Kali nethunter and FruityWiFi on the Nexus 5.

#### Reboot into fastboot:

    adb reboot bootloader

#### Unlock bootloader with:
    fastboot oem unlock

####Download the 6.0.1 image - hammerhead-mmb29v-factory-62a68ee9.tgz and flash with:
    flash-all

#### If fails due to space (from https://www.reddit.com/r/Android/comments/2m5ehy/workaround_for_failed_to_allocate_bytes_when/):
   

####Make sure the bootloader and radio succeeded
    Extract image-hammerhead-lrx21o.zip (if you're not using N5 it'll be named something slightly different)
####Run the following:
    fastboot flash boot boot.img
    fastboot flash recovery recovery.img
    fastboot flash system system.img
    fastboot flash cache cache.img

####If you want to wipe all your apps/data/pictures/etc then also type:
    fastboot flash userdata userdata.img

####Once those finish:
    fastboot reboot

###Setup phone
####Enable ADB debugging

####Push files to /sdcard:
    adb push SuperSU-v2.60-20151205163135.zip /sdcard/
    adb push nethunter-hammerhead-marshmallow-3.0.zip /sdcard/
    adb push anykernel2.zip /sdcard/

####Install CWM:
    adb reboot bootloader
    fastboot flash recovery  recovery-clockwork-touch-6.0.4.5-hammerhead.img

####Install SuperSu:
    Boot recovery
    Install SuperSU-v2.60-20151205163135.zip
    Reboot

####Install BusyBox (via Play Store)
####Install TWRP 2.8.7.1 (via Play Store)

####Reboot to TWRP

####Flash Nethunter - if it gets stuck on 70% - installing Kernel then:
    Hard reset
    Boot to recovery
    Install AnyKernel2.zip

####Upgrade NetHunter
Chroot may be missing(?)
Install Kali full chroot via Chroot Manager

In chroot:

    apt-get update
    apt-get upgrade

Install FruityWifi via http://fruitywifi.boards.net/thread/111/fruitywifi-v2-4

Install Intercepter-NG (via Play Store)
