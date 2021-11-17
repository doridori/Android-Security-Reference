# Platform Changes

- 4.4: Added to kernel. OEMs can enable
- 6: In 'Warning' mode
- 7: In 'Enforcing' mode
  - Phone wont boot if verified boot fails
    - Unless bootloader unlocked
      - No direct APIs to check bootloader status
      - [SafetyNet seems to now check bootloader lock status](https://www.reddit.com/r/Android/comments/587ss9/psa_android_safetynet_now_tripped_by_unlocking/)
      - [Key Attestion](https://developer.android.com/training/articles/security-key-attestation.html) checks bootloader status with hardware-based certs [verify source] signing the response
- 8: [Android Verified Boot](https://source.android.com/security/verifiedboot/)
  - Rollback prevention
- 9: Devices shipping with Android 9 MUST support verified boot [link](https://www.xda-developers.com/android-pie-rollback-protection/)

# Checking verified boot

- The [Android CDD](https://source.android.com/compatibility/7.0/android-7.0-cdd.html#9_10_device_integrity) states that if a device supports verified boot then it MUST set the `android.software.verified_boot` prop flag.
- [Key Attestion](https://developer.android.com/training/articles/security-key-attestation.html#certificate_schema) API can be used to check bootloader / verified boot status.
  - "Bootloader must provide Verified Boot public key and lock status to TEE" from [bootcamp](https://source.android.com/security/reports/Android-Bootcamp-2016-Android-Keystore-Attestation.pdf)
- There is a [kernal command line param](https://source.android.com/security/verifiedboot/verified-boot#bootloader_requirements) `androidboot.verifiedbootstate` (see `Communicating boot state`) but this can only be read by root [it seems](https://stackoverflow.com/questions/42719488/obtaining-kernel-command-line-parameters-of-android-linux-kernel)

# Overview

"Make persistence across reboots extraordinarily difficult"

> Verified Boot, introduced in Android 4.4, provides a hardware-based root of
trust, and confirms the state of each stage of the boot process. During boot,
Android warns the user if the operating system has been modified from the
factory version, provides information about what the warning means, and
offers solutions to correct it. Depending on device implementation, Verified
Boot will either allow the boot to proceed, stop the device from booting so
the user can take action on the issue, or prevent the device from booting up
until the issue is resolved. Starting from Android 6.0, device implementations
with Advanced Encryption Standard (AES) crypto performance above 50MiB/
seconds support Verified Boot for device integrity.

> Details on Android Verified Boot implementation and features can be found
in the [Verified Boot](https://source.android.com/security/verifiedboot/index.html) section on source.android.com.

_From [Android Security 2015 Year in Review](http://static.googleusercontent.com/media/source.android.com/en//security/reports/Google_Android_Security_2015_Report_Final.pdf)_



# Links

- [Verified Boot googlesource README](https://android.googlesource.com/platform/external/avb/+/master/README.md)
- [Signing boot images for Android Verified Boot (AVB)
](https://forum.xda-developers.com/android/software-hacking/signing-boot-images-android-verified-t3600606)
- [source.android.com] [https://source.android.com/security/verifiedboot/index.html](https://source.android.com/security/verifiedboot/index.html)
- [Verified boot in Android 7.0 won't let your phone boot if the software is corrupt](http://www.androidpolice.com/2016/07/20/verified-boot-android-7-0-wont-let-phone-boot-software-corrupt/)
- [Strictly Enforced Verified Boot with Error Correction](http://android-developers.blogspot.co.uk/2016/07/strictly-enforced-verified-boot-with.html)
- CTS doc [entry](https://source.android.com/compatibility/6.0/android-6.0-cdd.html#9_10_verified_boot) for M-6-23 
