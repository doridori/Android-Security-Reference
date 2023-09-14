# Keymaster

## Device Keymaster Version

The Keymaster implementation and its HAL seems to be tied to the version of Android the device shipped with, hence including this in the "hardware" section, even though strictly speaking it would be firmware.

At time of writing there are 4 Keymaster versions, spoken about briefly [here](https://source.android.com/docs/security/features/keystore).

## Misc.

The hardware `KeyStore` is accessed through an [OEM specfific HAL](https://source.android.com/security/keystore/). There is a `softkeymaster` also. See some interesting comments and links around this [here](https://doridori.github.io/android-security-the-forgetful-keystore/#comment-3220919933).

See [android.googlesource.com](https://android.googlesource.com/platform/system/keymaster/+/master) for `keymaster` code. This [clone](https://github.com/geekboxzone/mmallow_system_keymaster/) maybe easier to navigate.
