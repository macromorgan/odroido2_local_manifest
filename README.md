Local Manifests for Odroid Go Advance Android

This local manifests file pulls in an upstream version of Mesa, gbm_gralloc, drm_hwcomposer, and updates the version of libdrm.
In addition, you will have to manually tweak the following things in Mesa:

external/mesa3d/Android.mk - You will have to change the path from python and python3 to /usr/bin/python and /usr/bin/python3.
You might also need to install mako systemwide.  Mesa3d requires mako which isn't present in the AOSP prebuilts.

external/mesa3d/src/gallium/drivers/kmsro/Android.mk - add "rockchip" as one of the "GALLIUM_TARGET_DRIVERS".

After you successfully build the Android image, you will need to convert the ramdisk-recovery.img into a uInitrd by doing the following:

mkimage -A arm64 -O linux -T ramdisk -C none -n uInitrd -d ramdisk-recovery.img uInitrd

Use the uInitrd along with the prebuilt kernel and DTB files to boot.

Set up your partitions where there is 16MB free at the beginning of the drive, a boot partition that is FAT32 (your preference, probably at least 64MB ought to work).
Flash the u-boot from the device files to the SD card, and copy the kernel Image, boot.ini, dtbs, and uInitrd image to it.

Create 3 more partitions on the drive, a system partition of type EXT4 that is at least 1.5GB in size, a vendor partition that is of type EXT4 and at least 256MB in size, and a userdata partition that is of type EXT4 and the remaining space of your disk.

Use dd to copy the corresponding system.img to /dev/mmcblk0p2 (or whichever /dev node is the 2nd partition of your SD card), and vendor.img to /dev/mmcblk0p3 (or the 3rd partition of your SD card).
