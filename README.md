# Notes on the Duoqin F22 Pro

The Duoqin F22 Pro (aka. Qin F22 Pro) is an Android smartphone with a traditional numeric keypad and a small 3.54" multi-touch screen (640×960 pixels, 326 ppi) that can be imported from various sellers on AliExpress and Alibaba, some more shady than others.

![Duoqin F22 Pro](https://user-images.githubusercontent.com/36278767/203085989-1d1c3ab0-9bbc-43f3-90e5-73fee4c82904.jpg)

## Delivery

The following was in the box when I got it:

* 1x USB C cable (thrown away, because slightly moving the phone caused the USB connection to drop)
* 1x USB C earphones in a small plastic bag, the kind usually used for aftermarket accessories (untested)
* 1x cheap screen protector that doesn't fit properly (thrown away)
* SIM Tray removal tool
* The phone itself
* Transparent plastic case, which actually looks and feels okay

## Accessories

You cannot buy accessories for this phone outside of China. None of the popular places will sell you any because nobody is offering anything. AliExpress has some stuff on offer, but I decided to commission a custom 151mm x 61mm x 11mm felt sleeve for the phone from a local workshop that quoted a very reasonable price. This should fit the phone when its inside the plastic case.

## Key Combinations

* VOLUME UP + VOLUME DOWN + Plug in USB = Factory menu. All options and text are in Chinese.
* VOLUME UP + Plug in USB = Fastboot
* VOLUME DOWN + Plug in USB = MediaTek PreLoader

## Software

I bought the version that has Google Play preinstalled.

* It runs Android 12 with a security patch level of May 2022. The built-in updater doesn't find any updates. 
* The bootloader was unlocked
* It comes preloaded with the whole suite of Google's garbage. A cursory look showed that the apps seem to work. All Google apps can be disabled using [Universal Android Debloater](https://github.com/0x192/universal-android-debloater).
* Disabling all of the junk apps that are safe to disable leaves you with an almost pristine version of Android.
* The phone is not Google Play certified, so your banking or 2FA apps probably won't work with it.
* The keypad backlight doesn't work with this ROM.
* The keypad in general doesn't behave right with this ROM. To become useful, buttons need to be remapped using a third-party app.
* The phone can't be unlocked with just the keypad; you need to swipe the screen before being able to input the passcode.

The last point was a deal breaker for me.

The entire ROM feels like a quick hack that some third party quickly put together without testing it much. I commend the developer(s) for creating a clean ROM, but it once again confirmed my theory that Chinese developers see software as a means to an end, rather than taking pride in it. With Chinese hardware, the golden rule usually is to never update the firmware, because in 9.5 out of 10 cases, doing so will permanently brick your device. 

So obviously the best thing to do in this case is to change the firmware.

### Backup the Google Play ROM

You should backup the Google Play ROM the phone shipped with because chances are you will never get your hands on it again if things don't work out with the stock ROM. Please don't contact me about getting a copy of the ROM.

As is par for the course with Android phones, it's complicated and annoying. The following worked on Windows 10.

The process takes a long time, so you might want [download and run a stay awake tool](https://www.raymond.cc/blog/dont-sleep-prevent-windows-from-standby-shutdown-hibernate-and-restart/) to prevent Windows from putting your computer to sleep.

*The following process should be a read-only operation and should not cause damage to your phone, but if you continue, you are doing so at your own risk.*

1. Download the latest [MTKClient](https://github.com/bkerler/mtkclient)
1. Download the latest [UsbDk](https://github.com/daynix/UsbDk/releases/)
1. Download [Python](https://www.python.org/downloads/)
1. Download [Windows 10 MTK VCOM USB Preloader Drivers](https://www.thecustomdroid.com/mediatek-preloader-usb-vcom-drivers/)
1. Make a folder for backups. I used `D:\bk\`. The backup is huge. You should have >64GB free disk space.
1. Install the *Windows 10 MTK VCOM USB Preloader Drivers*. In my case this popped up a console window with it trying to remove nonpresent USB devices that seemed to have gotten stuck in an infinite loop. I let the console window scroll shit by me for about a minute before getting bored and closing it, which let the installer continue. It also wanted to reboot, which I refused to do.
1. Install *Python* (to make life easier, you can install it for all users and tell the installer to add Python to your %PATH%, but it's not strictly required)
1. Install *UsbDk*. Note that this can *disconnect your USB keyboard and mouse, leaving you without a means to control your system*. Windows needed a reboot in my case before all USB ports started working again. I was only able to do this via RDP (Device Manager said that Windows needed to reboot before it would be able to reload the driver for the USB Root Hub, which explains why USB completely died after installing UsbDk).
1. Extract *MTKClient* to a folder of your choice.
1. Remove your Google account from your phone to make sure Factory Reset Protection won't haunt you.
1. Make sure your phone is unplugged, then shut it down.
1. Open a command prompt and `cd` into the folder you extracted MTKClient into.
1. Run `pip3 install -r requirements.txt`
1. Run `python mtk rl D:\bk\` (adjust the path to backup directory as necessary).
1. You should get the message `Waiting for PreLoader VCOM, please connect mobile`. Now press and hold *VOLUME DOWN* on the phone, then plug in the USB C cable. Do not press any other buttons.
1. MTKClient will now run an exploit to give it control over the phone and then begins dumping the ROM, making one file for each part of it. The backup process will take anywhere from 6 – 8 hours, because it also includes the huge userdata partition for good measure. Just let it run over night.
1. Unplug the phone

### Install the Stock ROM

*If you follow these steps, you are doing so at your own risk. I wrote down the steps as I was doing them.*

The original Chinese + English stock ROM is apparently a little bit better. It doesn't include Google Play. But there's always F-Droid and many alternative stores. I got the ROM from a website called [ROM-Provider](https://romprovider.com/xiaomi-qin-f22-pro-firmware-support/). If you download the `ROM_2_1.0.7_Unloked.7z` file, and open it, inside will be a file named just `ROM_2`. You can open this file with 7-Zip and extract `42.super.img` from it.

1. Run `python mtk w 42.super.img` (adjust the path to backup directory as necessary).
1. You should get the message `Waiting for PreLoader VCOM, please connect mobile`. Now press and hold *VOLUME DOWN* on the phone, then plug in the USB C cable. Do not press any other buttons.
1. Wait for the process to complete, then unplug the phone.
1. Run `python mtk e metadata,userdata,md_udc` to reset the phone to factory defaults
1. You should get the message `Waiting for PreLoader VCOM, please connect mobile`. Now press and hold *VOLUME DOWN* on the phone, then plug in the USB C cable. Do not press any other buttons.
1. Wait for the process to complete, then unplug the phone.
1. Turn on the phone, it boots.

Have another phone with the Google Translate app ready and use the camera-based translator to understand the content of the screen, because the F22 Pro will now boot up in Chinese and ask you if you want to set up in student mode or normal mode. Use normal mode and make your way to the Settings app, where you can change the language to English.

While the phone appears to work better at first glance, you'll quickly find that Wireless LAN and Bluetooth are now broken. There's now also an over-the-air software update to version 1.1.0 available, which refuses to install correctly. Back to the command prompt.

#### Fix Wireless LAN and Bluetooth

1. Extract all the other images from the `ROM_2` file. Then delete these to preserve your NVRAM (IMEI, WiFi MAC, etc.): `nvcfg`, `nvdata`, `nvram`, `protect1`, `protect2`.
1. Run `python mtk wl "D:\chinarom"` -- replace the path as necessary.
1. Wait for the process to complete (roughly 30 minutes), then unplug the phone.
1. Run `python mtk e metadata,userdata,md_udc` to reset the phone to factory defaults
1. You should get the message `Waiting for PreLoader VCOM, please connect mobile`. Now press and hold *VOLUME DOWN* on the phone, then plug in the USB C cable. Do not press any other buttons. If you screw this up, you'll likely get two boot loops before Android tells you it can't boot; it then offers you to wipe userdata via the phone itself and you can skip the next two steps.
1. Wait for the process to complete, then unplug the phone.
1. Turn on the phone, it boots.

Wireless LAN and Bluetooth should now work and your phone should have cellular service as well.

If you try to run the software update to 1.1.0 again, it will still fail somewhere around 30%. This is why it fails:

```
11-21 15:08:51.856   910   910 I update_engine: [INFO:partition_writer_factory_android.cc(44)] Virtual AB Compression disabled, using Partition Writer for `preloader`
11-21 15:08:51.862   910   910 I update_engine: [INFO:partition_writer.cc(295)] Opening /dev/block/platform/bootdevice/by-name/preloader_b partition without O_DSYNC
11-21 15:08:51.867   910   910 I update_engine: [INFO:partition_writer.cc(100)] Caching writes.
11-21 15:08:51.873   910   910 I update_engine: [INFO:partition_writer.cc(307)] Applying 1 operations to partition "preloader"
11-21 15:08:51.890   910   910 E update_engine: [ERROR:fec_file_descriptor.cc(32)] No ECC data in the passed file
11-21 15:08:51.895   910   910 E update_engine: [ERROR:partition_writer.cc(608)] Unable to open ECC source partition preloader, file /dev/block/platform/bootdevice/by-name/preloader_a: Invalid argument (22)
11-21 15:08:51.900   910   910 E update_engine: [ERROR:delta_performer.cc(883)] The hash of the source data on disk for this operation doesn't match the expected value. This could mean that the delta update payload was targeted for another version, or that the source partition was modified after it was installed, for example, by mounting a filesystem.
11-21 15:08:51.905   910   910 E update_engine: [ERROR:delta_performer.cc(888)] Expected:   sha256|hex = 2A473B7FFBA22C06A4AAB2F7BE72B0FCB0051FECA41921AFBDA98B526B71FF12
11-21 15:08:51.910   910   910 E update_engine: [ERROR:delta_performer.cc(891)] Calculated: sha256|hex = 90E434E172E2DF246F0BFE354ACA97A7B8BB53492FE90D634288FAF2133DFDB0
11-21 15:08:51.914   910   910 E update_engine: [ERROR:delta_performer.cc(902)] Operation source (offset:size) in blocks: 0:76
11-21 15:08:51.919   910   910 E update_engine: [ERROR:partition_writer.cc(485)] source_fd != nullptr failed.
11-21 15:08:51.924   910   910 E update_engine: [ERROR:delta_performer.cc(957)] partition_writer_->PerformSourceBsdiffOperation( operation, error, buffer_.data(), buffer_.size()) failed.
11-21 15:08:51.928   910   910 E update_engine: [ERROR:delta_performer.cc(201)] Failed to perform BROTLI_BSDIFF operation 386, which is the operation 0 in partition "preloader"
11-21 15:08:51.933   910   910 E update_engine: [ERROR:download_action.cc(222)] Error ErrorCode::kDownloadStateInitializationError (20) in DeltaPerformer's Write method when processing the received payload -- Terminating processing
11-21 15:08:51.939   910   910 I update_engine: [INFO:delta_performer.cc(217)] Discarding 9222 unused downloaded bytes
```

I guess the preloader was replaced with a hacked version by the seller. OTA updates won't work until Qin pushes a full update rather than an incremental one. Lovely. :unamused:

### Backup Original Preloader

Use this command to create a backup of the preloader:

`python mtk r preloader "D:\bk\preloader.bin" --parttype=boot1`

You never know if you might need it in the future.

### Debloat the Chinese Stock ROM

Let's continue with debloating the China ROM.

Use [Universal Android Debloater](https://github.com/0x192/universal-android-debloater) for this.

**DO NOT DISABLE OR UNINSTALL `com.duoqin.qweather` OR THE STOCK LAUNCHER WILL STOP WORKING!**

Here's my debloat list:

```
com.android.bluetoothmidiservice
com.android.browser
com.android.calendar
com.android.calllogbackup
com.android.contacts
com.android.deskclock
com.android.dialer
com.android.dreams.basic
com.android.egg
com.android.gallery3d
com.android.mms
com.android.music
com.android.printservice.recommendation
com.android.quicksearchbox
com.android.sharedstoragebackup
com.android.simappdialog
com.android.soundrecorder
com.android.stk
com.android.traceur
com.android.wallpaperbackup
com.baidu.map.location
com.duoqin.ai
com.duoqin.app.guard
com.duoqin.duoqinaccount
com.duoqin.feedback
com.duoqin.guardapp
com.duoqin.inputmethod
com.duoqin.inputmethod.pinyin
com.duoqin.pinyin
com.duoqin.post
com.duoqin.remote
com.duoqin.remoteservice
com.duoqin.shortcuts
com.duoqin.syncassistant
com.duoqin.translator
com.marsqin.chat
com.mediatek.callrecorder
com.mediatek.mdmconfig
com.mediatek.mdmlsample
com.sprd.sprdcalculator
com.wapi.wapicertmanager
```

This leaves you with Camera, Files, Settings, the Latin IME (on screen keyboard), and an app with Chinese text that you can long tap and uninstall normally via the Android UI.

In the stock launcher, you'll still see the search bar, a Wechat box, and an Aipay box if you swipe all the way to the left. They are all broken and don't do anything though. If you tap on them, the launcher will pop up a message saying the app they need to function is missing.

### Next Steps

At this stage, consider installing [F-Droid](https://f-droid.org/), which you can do with ADB or via Bluetooth if you already have it on another phone. You can replace all the now gone stock apps with the ones from [Simple Mobile Tools](https://www.simplemobiletools.com/) via F-Droid. You even get the features limited to donators for free, if you install the "Simple Thank You" app. Consider making a donation to them if you enjoy their bloat-free and privacy-friendly replacements for stock apps.

To be continued...
