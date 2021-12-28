Its worth keeping in mind the nature of the below flaws, as they generally fall into camps such as:

- Needs physical (or adb) access
- Needs malicious app installed on the device 
- Needs access over digital cellular network
- MITM of comms of existing app on device

# Android platform

- Ashmem
  - [BitUnmap: Attacking Android Ashmem](https://googleprojectzero.blogspot.co.uk/2016/12/bitunmap-attacking-android-ashmem.html) _1st Dec 16_
    - Fixed in the Nov security update
- AutoFill
  - [FLAG_SECURE and Android O Autofill](https://commonsware.com/blog/2017/04/25/flag_secure-android-o-autofill.html) _25 Apr 17_
- Binder
  - [racy getpidcon usage permits binder service replacement](https://bugs.chromium.org/p/project-zero/issues/detail?id=851) _15th June 2016_ 
- Bluetooth
  - [Billions of devices imperiled by new clickless Bluetooth attack](https://arstechnica.com/information-technology/2017/09/bluetooth-bugs-open-billions-of-devices-to-attacks-no-clicking-required/) _13 Sept 17_
    - [Whitepaper](https://www.armis.com/blueborne/)
- Bootloader
  - [Mobile Bootloaders From Top Manufacturers Found Vulnerable to Persistent Threats](http://thehackernews.com/2017/09/hacking-android-bootloader-unlock.html) _Sept 17_
- Chipsets
  - [QuadRooter: New Android Vulnerabilities in Over 900 Million Devices](http://blog.checkpoint.com/2016/08/07/quadrooter/) _7th August 16_
    - [Full details](https://www.checkpoint.com/downloads/resources/quadRooter-vulnerability-research-report.pdf)
    - 3 of 4 of these are IOCTL based, and patched in August security patches
- Dev
  - [Unofficial Copies of Android Support Libraries Being Distributed on JCenter](http://tools.android.com/unofficial-copies-of-android-support-libraries-being-distributed-on-jcenter) _20 Sept 16_
  - [Reminder: Check Your Projects Before Importing Them](https://commonsware.com/blog/2016/09/19/reminder-check-projects-before-importing.html) _19 Sept 16_ 
- Drag & Drop
  - [Be Careful of Drag-and-Drop on Android N](https://commonsware.com/blog/2016/06/01/be-careful-drag-drop-android-n.html) _1st June 2016_
- Filesystem
  - [How My Rogue Android App Could Monitor & Brute-force Your App’s Sensitive Metadata](https://www.arneswinnen.net/2016/09/how-my-rogue-android-app-could-monitor-brute-force-your-apps-sensitive-metadata/) _8th Sept 16_
- FLAG_SECURE
  - [PSA: FLAG_SECURE Window Leaks](https://commonsware.com/blog/2016/06/06/psa-flag-secure-window-leaks.html)  _6 June 16_
- Intents
  - [Surreptitious Sharing on Android](https://www.ibr.cs.tu-bs.de/news/ibr/surreptitious-sharing-2016-04-04.xml) _4th April 2016_
    - Accessing app private data via intents
- Hardware 
  - [Shattered Trust: When Replacement Smartphone Components Attack](https://www.usenix.org/conference/woot17/workshop-program/presentation/shwartz) _Aug 17_
- Kernel 
  - [Dirty COW explained: Get a moooo-ve on and patch Linux root hole](http://www.theregister.co.uk/2016/10/21/linux_privilege_escalation_hole/) _21 Oct 16_
    - [What is the Dirty COW vulnerability and how does it impact mobile security?](https://www.nowsecure.com/blog/2016/10/21/dirty-cow-vulnerability-mobile-impact/)
    - [PoCs](https://github.com/dirtycow/dirtycow.github.io/wiki/PoCs)
- MediaServer
  - [Return to libstagefright: exploiting libutils on Android](https://googleprojectzero.blogspot.co.uk/2016/09/return-to-libstagefright-exploiting.html) _7th Sept 2016_
  - [EXIF Parsing](http://www.forbes.com/sites/thomasbrewster/2016/09/06/google-android-one-photo-hack/#3db069111555) _7th Sept 16_ ([fix](https://twitter.com/timstrazz/status/773275505235591168))
  - StageFright
    - [Hardening the media stack](http://android-developers.blogspot.co.uk/2016/05/hardening-media-stack.html) _5th May 16_
    - [StageFrightened](http://googleprojectzero.blogspot.co.uk/2015/09/stagefrightened.html) _16th Sept 15_
- Media Transfer Protocol (MTP)
  - [MTPwn](https://github.com/smeso/MTPwn) _Jan 18_
  - Private app files seem to be ok
- Mem Dumps
  - [Undocumented Patched Vulnerability in Nexus 5X Allowed for Memory Dumping Via USB](https://securityintelligence.com/undocumented-patched-vulnerability-in-nexus-5x-allowed-for-memory-dumping-via-usb/) _1st Sept 2016_  
  - [Forensics tool nabs data from Signal, Telegram, WhatsApp ](http://www.theregister.co.uk/2016/08/15/retroscope/?mt=1471266388161) _15h Aug 2016_
- OpenSSL
  - [Heartbleed](https://en.wikipedia.org/wiki/Heartbleed)
    - Android 4.1.1 only as mentioned in link
- Samsung
  - [Remote Code Execution as System User on Samsung Phones](https://www.nowsecure.com/blog/2015/06/16/remote-code-execution-as-system-user-on-samsung-phones/)
- TapJacking
  - `SYSTEM_ALERT_WINDOW`
    - System alert windows can only either consume or pass-on motion events based upon its `Window` bounds
      - A single overlay can have multiple `Windows`
    - [SYSTEM_ALERT_WINDOW, Android O, and Disappointment](https://commonsware.com/blog/2017/05/11/system_alert_window-updates.html) _11th May 17_
    - [Google Working on Fix for Android Permission Weakness](https://www.onthewire.io/google-working-on-fix-for-android-permission-weakness/)
    - [Android's Hover feature is a data HOOVER](http://www.theregister.co.uk/2016/11/08/androids_hover_/) _8th Nov 2016_
    - [How Tapjacking Made a Return with Android Marshmallow — and Nobody Noticed](https://www.xda-developers.com/how-tapjacking-made-a-return-with-android-marshmallow-and-nobody-noticed/)
    - [‘SAW’-ing through the UI: Android overlay malware and the System Alert Window permission explained](https://www.nowsecure.com/blog/2017/05/25/android-overlay-malware-system-alert-window-permission/)
    - Restricted touch functionality in 4.0 [SO](https://stackoverflow.com/a/9462190/236743)
    - `FLAG_NOT_FOCUSABLE` will allow all touch to pass through, otherwise all consumed (needs confirmation)
    - [Perm deprecated in Q](https://twitter.com/reyammer/status/1133785544209698816)
  - `BIND_ACCESSIBILITY_SERVICE`
    - [How Android Accessibility Services Can Be Used To Hack Your Phone](http://www.makeuseof.com/tag/android-accessibility-services-can-used-hack-phone/) _17th May 2016_
      - Up to M-6-23 only as takes advantage of gaps in system overlays
    - [On Malware Leveraging the Android
Accessibility Framework](http://www.cs.uml.edu/~xinwenfu/paper/Accessibility.pdf) _~2013_
      - Uses Accessibility Framework to detect launcher icon presses and will show fake application instead. 
  - `Both`
      - [CLOAK & DAGGER: FROM TWO PERMISSIONS TO COMPLETE CONTROL OF THE UI FEEDBACK LOOP](https://www.blackhat.com/us-17/briefings/schedule/index.html#cloak--dagger-from-two-permission-to-complete-control-of-the-ui-feedback-loop-6210) _22 Jul 17_
        - [cloak-and-dagger.org](http://cloak-and-dagger.org/)
  - `TYPE_APPLICATION_OVERLAY`
    - No Vulns yet tracked
    - [New in Android O](https://developer.android.com/about/versions/oreo/android-8.0-changes.html#all-aw)
- Signing
  - [Android Fake ID Vulnerability](https://www.blackhat.com/docs/us-14/materials/us-14-Forristal-Android-FakeID-Vulnerability-Walkthrough.pdf)
  - [Janus](https://www.guardsquare.com/en/blog/new-android-vulnerability-allows-attackers-modify-apps-without-affecting-their-signatures?utm_content=buffer9737f&utm_medium=social&utm_source=twitter.com&utm_campaign=buffer) _Dev 17_
    - APK and dex file duality
- TCP
  - [Linux flaw that allows anyone to hijack Internet traffic also affects 80% of Android devices](https://blog.lookout.com/blog/2016/08/15/linux-vulnerability-android/) _15th Aug 16_
- TEE
  - [Technical Advisory: Private Key Extraction from Qualcomm Hardware-backed Keystores](https://www.nccgroup.trust/us/our-research/private-key-extraction-qualcomm-keystore/?research=Technical+advisories)
  - [34C3 - Microarchitectural Attacks on Trusted Execution Environments](https://www.youtube.com/watch?v=G8-3G_cep4M)
- TLS
  - [Studying TLS Usage in Android Apps](https://people.cs.umass.edu/~phillipa/papers/conext2017_tls_paper.pdf)
- TrustZone
  - [CLKSCREW: Exposing the perils of security-oblivious energy management](https://blog.acolyer.org/2017/09/21/clkscrew-exposing-the-perils-of-security-oblivious-energy-management/) _SEPTEMBER 21, 2017_
  - [Extracting Qualcomm's KeyMaster Keys - Breaking Android Full Disk Encryption](https://bits-please.blogspot.co.uk/2016/06/extracting-qualcomms-keymaster-keys.html?m=1) _30th June 16_
- Uninstallation
  - [Life after App Uninstallation: Are the Data Still
Alive? Data Residue Attacks on Android](http://www.cis.syr.edu/~wedu/Research/paper/data_residue_ndss2016.pdf) _2016_
- Zygote
  - [Attack on Zygote: a new twist in the evolution of mobile threats](https://securelist.com/analysis/publications/74032/attack-on-zygote-a-new-twist-in-the-evolution-of-mobile-threats/) _3rd March 2016_
    - One of the best articles I have seen deconstructing malware 

# Dependencies / Populat libs

- Log4j
  - [Log4Shell: RCE 0-day exploit found in log4j 2, a popular Java logging package](https://www.lunasec.io/docs/blog/log4j-zero-day/) _Dec 2021_
    
# Hardware

- Mobile
  - [Power to peep-all: Inference Attacks by Malicious Batteries on Mobile Devices](https://sites.google.com/site/silbersteinmark/Home/popets18power.pdf)
- Android 
  - [Every Android device is susceptible to a hardware vulnerability called RAMpage](https://www.xda-developers.com/android-hardware-vulnerability-rampage/)
- CPU
  - https://cpu.fail/
