- [Difference between TEE and SE](https://security.stackexchange.com/questions/122738/difference-between-tmp-tee-and-se)
- [Accessing the embedded secure element in Android 4.x 
](https://nelenkov.blogspot.co.uk/2012/08/accessing-embedded-secure-element-in.html)
  - Not open to 3rd party apps. OS signed apps _may_ be able to access the UICC/SIM SE.
- [Using the SIM card as a secure element in Android 
](https://nelenkov.blogspot.co.uk/2013/09/using-sim-card-as-secure-element.html)
  - The AOSP version of Android does not provide a standard API to use the SIM card as a SE, but many vendors do, and as long as the device baseband and RIL support APDU exchange, one can be added by using the SEEK for Android patches. This allows to improve the security of Android apps by using the SIM as a secure element and both store sensitive data and implement critical functionality inside it. Commercial SIM do not allow for installing arbitrary user applications, but applets can be automatically loaded by the carrier using the SIM OTA mechanism and apps that take advantage of those applets can be distributed through regular channels, such as the Play Store.
- [Host-based Card Emulation](https://developer.android.com/guide/topics/connectivity/nfc/hce.html)
  - Alternative to SE based card-emulation

# Devices / OEMs

## Pixel 2

The Pixel 2 ships with a [hardware security module](https://www.blog.google/products/android-enterprise/how-pixel-2s-security-module-delivers-enterprise-grade-security/) which contains an [NXP](https://plus.google.com/+DeesTroy/posts/i33ygUi7tiu) discrete [chip](https://www.ifixit.com/Teardown/Google+Pixel+2+XL+Teardown/98093#s180076), which also [slows down the rooting process](https://www.xda-developers.com/magisk-v14-4-root-pixel-2-xl-su/). 

## Pixel 3 and Titan

[Building a Titan: Better security through a tiny chip](https://android-developers.googleblog.com/2018/10/building-titan-better-security-through.html)

## Pixel 6 and Titan M2 & Tensor Processor

...

## Samsung

- [eSE](https://github.com/doridori/Android-Security-Reference/blob/master/hardware/SE.md) / [sdk](https://developer.samsung.com/ese/overview.html)
