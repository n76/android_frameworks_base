android_frameworks_base
=======================

This is a fork of the android_frameworks_base from the [TI OMAP4 AOSP](https://github.com/OMAP4-AOSP) project. That project provides a generic Android 6 (Marshmallow) ROM for a number of devices that used the TI OMAP4 chip including the Galaxy Nexus.

This fork adds a new aosp-6.0-microg branch which tracks the aosp-6.0 branch but is [patched to support signature faking](https://github.com/microg/android_packages_apps_GmsCore/tree/master/patches) needed to run the [microG project's](http://forum.xda-developers.com/android/apps-games/app-microg-gmscore-floss-play-services-t3217616) [GmsCore component](https://github.com/microg/android_packages_apps_GmsCore).

Building a ROM
==============

To build a AOSP 6.0.x ROM for a supported device the following steps are needed:

-	Create a local_manifest.xml file containing the following:
```
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
  <remote  name="n76"
           fetch="https://github.com/n76" />

  <!-- Support microG with fake signature permission -->
  <remove-project path="frameworks/base" name="android_frameworks_base" />
  <project path="frameworks/base" name="android_frameworks_base" remote="n76" revision="aosp-6.0-microg" groups="pdk-cw-fs,pdk-fs" />

</manifest>
```
-	Setup the build tree:
```
mkdir OMAP4
cd OMAP4/
repo init -u git://github.com/OMAP4-AOSP/android.git -b aosp-6.0
mkdir .repo/local_manifests
cp ~/local_manifest.xml .repo/local_manifests/
repo sync
```
-	Make the ROM
```
. build/envsetup.sh
# in lunch select number for aosp_tuna-userdebug
lunch
make installclean
make clean
make -j2 otapackage 2>&1 | tee ~/build.log
```
