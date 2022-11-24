# Notes on the Duoqin F22 Pro

The Duoqin F22 Pro (aka. Qin F22 Pro) is an Android smartphone with a traditional numeric keypad and a small 3.54" multi-touch screen (640×960 pixels, 326 ppi) that can be imported from various sellers on AliExpress and Alibaba, some more shady than others.

![Duoqin F22 Pro](https://user-images.githubusercontent.com/36278767/203085989-1d1c3ab0-9bbc-43f3-90e5-73fee4c82904.jpg)

As of November 2022, I use this as my primary phone. Excessive smartphone use is harmful, bad for mental health, and associated with physical health problems. This Duoqin F22 Pro, while offering a complete Android experience, is annoying enough that you end up not wanting to use it very much, yet it can do maps, browsing, email, messaging, and whatever else, if it's really urgent. Perfect!

## Delivery

The following was in the box when I got it:

* 1x USB C cable (thrown away, because slightly moving the phone caused the USB connection to drop)
* 1x USB C earphones in a small plastic bag, the kind usually used for aftermarket accessories (untested)
* 1x cheap screen protector that doesn't fit properly (thrown away)
* SIM Tray removal tool
* The phone itself
* Transparent plastic case, which actually looks and feels okay

## Accessories

You cannot buy accessories for this phone outside of China. None of the popular places will sell you any because nobody is offering anything. AliExpress has some stuff on offer, but I decided to commission a custom 151mm x 61mm x 11mm felt sleeve for the phone from a local workshop that quoted a very reasonable price. Unfortunately it turned out too tight to fit the phone when its in the plastic case, but it fits the phone perfectly without it. 151mm x 65mm x 20mm might be better for a sleeve that can fit the plastic case. Since the case is anti-slip the sleeve should not be a tight fit.

## Key Combinations

* VOLUME UP + VOLUME DOWN + Plug in USB = Factory menu. All options and text are in Chinese.
* VOLUME UP + Plug in USB = Fastboot
* VOLUME DOWN + Plug in USB = MediaTek PreLoader

## Software

There seem to be three versions of this phone:
1. Original from the factory: Chinese + English, without Google Play
2. Original from the factory, hacked: Chinese + English, OEM unlocked, Google Play and hacked to think the phone is certified by Google
3. International ROM with Google Play: Supports many languages, is OEM unlocked, has Google Play, is not certified by Google

I received the third flavor; the version that has Google Play preinstalled and supports languages other than Chinese and English.

* It runs Android 12 with a security patch level of June 2022. The built-in updater doesn't find any updates. 
* The bootloader was unlocked
* It comes preloaded with the whole suite of Google's garbage. A cursory look showed that the apps seem to work. All Google apps can be disabled using [Universal Android Debloater](https://github.com/0x192/universal-android-debloater).
* Disabling all of the junk apps that are safe to disable leaves you with an almost pristine version of Android.
* The phone is not Google Play certified, so your banking or 2FA apps probably won't work with it.
* The keypad backlight doesn't work with this ROM.
* The keypad in general doesn't behave right with this ROM. To become useful, buttons need to be remapped using a third-party app.
* ~~The phone can't be unlocked with just the keypad~~ Press the top left button (the one with the strange symbol that looks like a badly drawn heart or cat ears with pierced lip) to trigger PIN entry, then use the number keys as expected.
* Turning off the screen with the power button and immediately turning it back on has a 50% chance of a black flickering screen appearing. Workaround is to turn the screen back off, wait one second, and then turn it on.

The entire ROM feels like a quick hack that some third party quickly put together without testing it much. I commend the developer(s) for creating a clean ROM, but it once again confirmed my theory that Chinese developers see software as a means to an end, rather than taking pride in it. With Chinese hardware, the golden rule usually is to never update the firmware, because in 9.5 out of 10 cases, doing so will permanently brick your device. 

So obviously the best thing to do in this case is to change the firmware to the first flavor. :weary:

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

#### Do Not Remove
* `com.duoqin.qweather` (the stock launcher will stop working; the app itself is garbage and only supports some Chinese cities)
* `com.duoqin.shortcuts` (setting "Quick Key Settings > Keys simulate touch-screen..." breaks)


#### Probably Safe to Remove
Here's my debloat list:

```
com.android.bluetoothmidiservice
com.android.browser
com.android.calendar
com.android.calllogbackup
com.android.contacts
com.android.deskclock
com.android.dialer
com.android.documentsui
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
com.duoqin.syncassistant
com.duoqin.translator
com.marsqin.chat
com.mediatek.callrecorder
com.mediatek.mdmconfig
com.mediatek.mdmlsample
com.sprd.sprdcalculator
com.wapi.wapicertmanager
```

This leaves you with:
* Camera
* Settings
* Weather
* Latin IME (default English on-screen keyboard)
* An app with Chinese text that you can long tap and uninstall normally via the Android UI

In the stock launcher, you'll still see the search bar, a Wechat box, and an Aipay box if you swipe all the way to the left. They are all broken and don't do anything though. If you tap on them, the launcher will pop up a message saying the app they need to function is missing.

### Chinese Stock ROM Bugs

#### WiFi Calling Cannot be Turned On

WiFi Calling can't be enabled in firmware release 1.0.7 due to a bug. The settings app crashes if you try, because the developers screwed up:

```
11-22 18:35:46.811  4625  4625 D AndroidRuntime: Shutting down VM
11-22 18:35:46.812  4625  4625 E AndroidRuntime: FATAL EXCEPTION: main
11-22 18:35:46.812  4625  4625 E AndroidRuntime: Process: com.android.settings, PID: 4625
11-22 18:35:46.812  4625  4625 E AndroidRuntime: java.lang.RuntimeException: Unable to start activity ComponentInfo{com.android.settings/com.android.settings.Settings$WifiCallingSettingsActivity}: java.lang.IllegalStateException: This Activity already has an action bar supplied by the window decor. Do not request Window.FEATURE_ACTION_BAR and set android:windowActionBar to false in your theme to use a Toolbar instead.
11-22 18:35:46.812  4625  4625 E AndroidRuntime:        at android.app.ActivityThread.performLaunchActivity(ActivityThread.java:3675)
11-22 18:35:46.812  4625  4625 E AndroidRuntime:        at android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:3832)
11-22 18:35:46.812  4625  4625 E AndroidRuntime:        at android.app.servertransaction.LaunchActivityItem.execute(LaunchActivityItem.java:103)
```
Launching it via shell also fails:

`am start -a android.intent.action.MAIN -n com.android.settings/.Settings\$WifiCallingSettingsActivity`

The settings app flashes up for a second and disappears. The error is the same as above. This can't be fixed without rooting the phone and patching the Settings APK. Maybe a newer firmware fixes it. I can't test that since the phone won't install OTA updates right now as described above.

#### Lock Screen Insecure

The lock screen only supports 
* No security
* Pattern unlock
* PIN unlock

Password locking is completely unavailable and the PIN unlock is limited to four digits. Four digit PINs can be guessed easily based on fingerprints you leave either on the touch screen or on the keypad, and it is strongly recommended to use at least six digit PINs.

I assume that restricting the PIN to four digits is to help law enforcement in China make sure that people follow the rules set by the General Secretary of the Communist Party.

![Pleasing to back click away for not government review](https://user-images.githubusercontent.com/36278767/203406312-27ef91f7-9168-4fa7-9bf6-f9027ce7279a.png)

### Next Steps

At this stage, and if you can live with the bugs and limitations, consider installing [F-Droid](https://f-droid.org/), which you can do with ADB or via Bluetooth if you already have it on another phone. You can replace all the now gone stock apps with the ones from [Simple Mobile Tools](https://www.simplemobiletools.com/) via F-Droid. You even get the features limited to donators for free, if you install the "Simple Thank You" app. Consider making a donation to them if you enjoy their bloat-free and privacy-friendly replacements for stock apps.

### Back to the ROM the Phone Shipped With

I want to like this phone, but it's difficult choosing between a sloppy, unfinished international ROM with bugs and the stock ROM with other bugs.

Anyway, going back to the previous ROM is also done using the MTKClient tool. If you still have your ROM backup, that is.

1. Make sure your phone is unplugged, then shut it down.
1. Open a command prompt and `cd` into the folder you extracted MTKClient into.
1. Run `python mtk wl D:\bk\` (adjust the path to backup directory as necessary).
1. You should get the message `Waiting for PreLoader VCOM, please connect mobile`. Now press and hold *VOLUME DOWN* on the phone, then plug in the USB C cable. Do not press any other buttons.
1. Wait a bit and you should be back on the old ROM.

#### Google ROM Debloat

Again, use [Universal Android Debloater](https://github.com/0x192/universal-android-debloater) for this.

Here's my list:
```
### Disable ###

# Need to enable temporarily when setting up Android Auto
com.google.android.apps.googleassistant 

# Will crash Settings app if uninstalled
com.google.android.apps.wellbeing

### Uninstall ###

# Warning: Do not do this if you have no other keyboards installed!
# If you use password lock, you won't be able to unlock your phone after reboot!
# If you use PIN or pattern lock, they work fine even if this is uninstalled.
com.google.android.inputmethod.latin

# Warning: Uninstall breaks Settings > Storage > Free up space 
# "No Activity found to handle Intent { act=android.os.storage.action.MANAGE_STORAGE }"
com.google.android.apps.nbu.files

# Warning: Breaks search box in app drawer of the default launcher (QuickStep)
com.google.android.googlequicksearchbox

com.android.bluetoothmidiservice
com.android.calllogbackup
com.android.chrome
com.android.deskclock
com.android.dreams.basic
com.android.egg
com.android.nfc
com.android.providers.partnerbookmarks
com.android.sharedstoragebackup
com.android.simappdialog
com.android.stk
com.android.traceur
com.duokan.phone.remotecontroller
com.google.android.apps.messaging
com.google.android.apps.photos
com.google.android.apps.restore
com.google.android.as
com.google.android.calendar
com.google.android.contacts
com.google.android.dialer
com.google.android.ext.shared
com.google.android.feedback
com.google.android.gm
com.google.android.gms.location.history
com.google.android.marvin.talkback
com.google.android.onetimeinitializer
com.google.android.partnersetup
com.google.android.printservice.recommendation
com.google.android.syncadapters.contacts
com.google.android.tag
com.google.android.tts
com.mediatek.mdmconfig
```

This should leave nothing except Camera, Play Store, and Settings. All the essential apps like the dialer, SMS, etc. can be replaced with better alternatives from Simple Mobile Tools (see "Next Steps" above).

## Japanese Input (Bonus: Romaji T9 Input)

For certain reasons, I need a Japanese IME on this phone. Unfortunately there are no easily available solutions for Japanese input on a phone like this. It's possible to use the touch screen, but using the hardware keys for 10 key kana input (テンキー入力) would be a lot nicer.

In Japan, a company called Freetel released a strange flip phone called Musashi FTJ161A in 2016. That phone was really bad. It was very expensive, but it had poor build quality, poor hardware specs, poor and outdated software on launch, and the camera was absolutely terrible. It didn't even have a headphone jack. And yes, I owned one. :disappointed:

The only novelty was that it contained two touch screens, so you could operate it opened or closed.

Originally, Freetel provided a quickly thrown together version of the Open Wnn IME for Japanese, but after enough complaints from Japanese customers, Freetel released a firmware update that included a special version of a Japanese IME called FSKAREN by FUJISOFT. FSKAREN is not very good when compared to modern Google Japanese IME with cloud suggestions, but it's better than nothing.

A ROM dump containing the special version of FSKAREN is still available online, so let's see what can be done.

### Get the Freetel Musashi ROM

You need a Linux system available for the following steps.

1. Download the ROM: [FTJ161A ROM](https://www.romgsm.net/2020/03/freetel-musashi-ftj161a-mt6735-firmware.html). The password is "Musashi FTJ161A"
1. Extract the ZIP file to a convenient location. 
1. The `system.bin` file is a normal ext4 partition. You can mount it on Linux: `mkdir /tmp/freetel && mount system.img /tmp/freetel`.
1. Take the APK file from `/tmp/freetel/app/FSKAREN_3.22FT01005/FSKAREN_3.2.2FT01005.apk`
1. (Optional) Also take the APK file from `/tmp/freetel/app/LatinIME/LatinIME.apk` if you want; it seems to be a patched version of the AOSP romaji keyboard that supports T9. Inside the APK are T9 dictionaries for many languages.

### Patch the APK

The problem is that FSKAREN checks `mtek_watchdog_flip_status` to set a value called `mFlipOpened` depending on whether the phone was opened or closed.  This doesn't exist on the Duoqin F22 Pro, so by default FSKAREN will always think the phone is closed and will display a normal touchscreen keyboard. The APK needs to be patched so that two checks for the value of mFlipOpened are bypassed. FSKAREN will then think the phone is always flipped open and only operate in ten key kana input (テンキー入力) mode.

Luckily the developer who implemented the changes for the Freetel Musashi made them open source under the Apache License, Version 2.0. Well, sort of, and probably by mistake. Inside the APK you will find the file `jp\co\fsi\fskaren\pom\FSIME.java.bak` which contains the full source code of the `FSIME` class. Search for `MUSASHI用開閉フラグ` in that file and you will immediately get an idea about what needs to be done next. :smiley:

You need these tools to proceed:
* [Oracle JDK](https://www.oracle.com/java/technologies/downloads/)
* [apktool](https://bitbucket.org/iBotPeaches/apktool/downloads/)
* [smali and baksmali](https://bitbucket.org/JesusFreke/smali/downloads/)
* `zipalign.exe` from the Android SDK

#### Decompile the APK

1. Open a command prompt
1. Decompile `FSKAREN_3.2.2FT01005.apk` using the command `apktool d FSKAREN_3.2.2FT01005.apk`. Then you get a new folder called `FSKAREN_3.2.2FT01005`.
1. Open `FSKAREN_3.2.2FT01005\smali\jp\co\fsi\fskaren\pom\FSIME.smali` with a text editor
1. Search for `mtek_watchdog_flip_status`, you should find it on line 23,864.
1. On line 23,872 change `if-lez v7, :cond_4` to `# if-lez v7, :cond_4`
1. Search again for `mtek_watchdog_flip_status`, you should find it on line 29,325.
1. On line 29,333 change `if-lez v3, :cond_3` to `# if-lez v3, :cond_3`
1. Close your editor; you've just patched out the jump instructions that trigger activating to the touchscreen keyboard

#### Recompile the APK

1. In the command prompt, `cd` into `FSKAREN_3.2.2FT01005`
1. Run `apktool b` to rebuild the APK file. It should return no errors.
1. Run `keytool -genkey -v -keystore key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias fskaren` and answer all the prompts (keytool is in the JDK program folder)
1. Run `jarsigner -keystore key.jks fskaren.apk fskaren` (jarsigner is in the JDK program folder)
1. Run `zipalign -v 4 FSKAREN_3.2.2FT01005\dist\FSKAREN_3.2.2FT01005.apk fskaren.apk`

Now you can sideload `fskaren.apk` using onto the Duoqin F22 Pro using `adb install` or whatever method you prefer. The phone will complain that it was built for an old version of Android, but if you confirm all of those nag screens, it will work perfectly. Even kanji candidate selection using the center navigation button works without problems.

#### What about LatinIME?

I use [Traditional T9](https://codeberg.org/HenriDellal/TraditionalT9) ([compiled APK](https://web.archive.org/web/20200919201626/https://github.com/HenriDellal/TraditionalT9/releases/download/20190315/t9.apk)), but if you want to try Freetel's hacked LatinIME with T9 support, it can be patched as well. It includes these dictionaries:

* t9_pl.dict
* t9_pt_br.dict
* t9_pt_pt.dict
* t9_ro.dict
* t9_ru.dict
* t9_sl.dict
* t9_sr.dict
* t9_sv.dict
* t9_tr.dict
* t9_cs.dict
* t9_da.dict
* t9_de.dict
* t9_el.dict
* t9_en.dict
* t9_en_gb.dict
* t9_en_us.dict
* t9_es.dict
* t9_fi.dict
* t9_fr.dict
* t9_hr.dict
* t9_it.dict
* t9_iw.dict
* t9_lt.dict
* t9_lv.dict
* t9_nb.dict
* t9_nl.dict

If you got `LatinIME.apk` from the Freetel Musashi firmware, the patch process is almost exactly the same. :smiley:

The file you need to edit is called: `LatinIME\smali\com\android\inputmethod\latin\LatinIME.smali`. On line 3652, change `if-nez v0, :cond_0` to `goto :cond_0` to turn the conditional jump into an unconditional jump, which forces T9 mode to be always on. Then recompile as described above. You will get a few warning messages, but the APK will work.

Note that T9 will only work in some input fields. It doesn't work in the search box of the Settings app, but it does work on the search box of F-Droid, for example. Active T9 will be indicated at the bottom: Instead of "English - abc" it will say "English - **S**abc".

### Ready to Use APKs

I have uploaded the two APKs that I patched using the above process to this repository. If you don't trust me, please recreate them yourself.

## Remapping Buttons Without Root

There is no dedicated backspace key, which is highly annoying when typing using T9 or 10key kana. I therefore remapped the back button to also act as a backspace key. I also remapped the center (round) button to act as an Enter key. This is how I did it:

1. Install [Shizuku](https://shizuku.rikka.app/)
1. Install [Key Mapper](https://f-droid.org/de/packages/io.github.sds100.keymapper/)
1. Go through the step by step setup of both apps so that they stop complaining and start working; the process is documented well in each app
1. In Key Mapper, tap the big **+** button, then tap the red **Record Trigger** button, and press the round center button on the phone
1. Next, in **Actions**, tap **Add action**, then tap **Input key code**
1. Select *66 KEYCODE_ENTER*
1. Tap the little floppy disk icon at the bottom right to save
1. Tap the big **+** button again, then record a trigger for the physical back button (the top right button with the u-turn arrow)
1. Ignore the warning that pops up, then, in **Actions**, tap **Add action**, and then tap **Input key code**
1. Select *67 KEYCODE_DEL*
1. Save the mapping like before
1. Tap the big **+** button yet again, then record another trigger for the physical back button
1. This time, don't add a key code trigger, but scroll down in the list of triggers and select **Go back**
1. Save the mapping like before
1. Edit the last mapping you just added by tapping on its entry, then select **Long press** at the bottom and save

It should look like this:

![Mapped buttons](https://user-images.githubusercontent.com/36278767/203872279-5ff326b5-30a0-46df-a174-da5de4499da5.png)

Now you can confirm search and URL boxes, insert new lines, etc. by pressing the round center button on the phone. If you short press the back button, it will delete the last character you entered. If you long press it, it will act like it did before and go back or exit out of an app.

The downside of this approach is that Shizuku has to be started again on every reboot and requires debugging permissions to be always enabled (somewhat dangerous). The alternative is using an invisible virtual keyboard provided by the Key Mapper app, but this is extremely awkward and doesn't work right. The other alternative is rooting the phone, which breaks apps that check for root unless you start playing cat and mouse games with Magisk. To summarize: everything sucks.
