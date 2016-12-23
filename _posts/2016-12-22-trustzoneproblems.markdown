---
layout: post
title: Resolving This package is for "OnePlus" devices this is a "onyx"
date:   2016-12-10 14:50:45 +0530
categories: android
---
Background
--------------------
I recently ran into this error which said "This package is for "OnePlus" devices this is a "onyx"". So searched the zip for that error message and i found it in `{unzippedfolder}/META-INF/com/google/android/update-script`.
There it was the assertion, this was probably done as a safety measure so you don't flash zip that wasn't designed for your device. `getprop("ro.build.product")` is how the script gets your device name. Now, there exists a build.prop file located in `/system/`, from which getprop() gets your `ro.build.product`.

The Fix
--------------------
### Option 1
*Step 1:*  
Unzip the flashable zip.  
*Step 2:*  
Get into the folder and open `META-INF/com/google/android/update-script` in an editor.  
*Step 3:*  
Remove the assertions. I had to delete this in my script.  
`getprop("ro.build.product") == "OnePlus" || abort("This package is for \"OnePlus\" devices; this is a \"" + getprop("ro.build.product") + "\".");`  
*Step 4:*  
Zip the folder containing the changes and flash it.  
### Option 2

Running `adb pull /system/build.prop` will get you a copy of `build.prop` on your Computer. Keep a backup incase you screw up. Now edit this carefully so that you only edit `ro.build.product` to whatever your zip expects it to be. Now you have to copy this back to the device, `adb push build.prop /system/`.Next, run these in order, `adb shell` `cd system` `chmod 644 build.prop`. Done. Now you should be able to flash your zip.


That's all folks!!
=====================
