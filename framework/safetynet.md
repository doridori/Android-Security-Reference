_Deprecated and superceeded by [Play Integrity API](https://github.com/doridori/Android-Security-Reference/blob/master/framework/play_integrity_api.md)_

# SafetyNet

- SafetyNet Attestation API
- SafetyNet Verify Apps API
- SafetyNet reCAPTCHA API
- SafetyNet Safe Browsing API

## What does it do?

- [developer.android.com] [Checking Device Compatibility with SafetyNet](https://developer.android.com/training/safetynet/index.html)
  - (about core function) "The service provides an API your app can use to analyze the device where it is installed. The API uses software and hardware information on the device where your app is installed to create a profile of that device. The service then attempts to match it to a list of device models that have passed Android compatibility testing. This check can help you decide if the device is configured in a way that is consistent with the Android platform specifications and has the capabilities to run your app."

## What does it look for

- Bootloader lock status [(reddit)](https://www.reddit.com/r/android/comments/587ss9/_/)
- And more (see above links)

## Verified Apps checking

Undocumented at present but can:

- `SafetyNet.SafetyNetApi.isVerifyAppsEnabled(context);`
- `SafetyNet.SafetyNetApi.enableVerifyApps(googleApiClient);`

## Signing of the attestation 

Likely performed on Googles servers.

## Google Play device exclusion rules

- [Netflix was just the start: Google Play Console lets developers exclude app availability for devices that don't pass SafetyNet](http://www.androidpolice.com/2017/05/18/netflix-just-start-google-play-console-lets-developers-exclude-app-availability-devices-dont-pass-safetynet/)
- [Netflix's SafetyNet Exclusion is Actually a New Feature in the Google Play Console](https://www.xda-developers.com/netflixs-safetynet-exclusion-is-actually-a-new-feature-in-the-google-play-console/)

## Links

- [xda-developers.com] [Google Security Engineer Explains Issues With Root and Android Pay in the XDA Forums](http://www.xda-developers.com/google-security-engineer-explains-issues-with-root-and-android-pay-in-the-xda-forums/) 
- [Inside SafetyNet series](https://koz.io/inside-safetynet-3/)
- [[Guide] Bypass Safetynet on MM with Custom ROM & Kernel](https://forum.xda-developers.com/galaxy-s6/general/guide-bypass-safetynet-mm-custom-rom-t3534709)
- [API docs](https://developers.google.com/android/reference/com/google/android/gms/safetynet/SafetyNetApi)
- [Inside Android’s SafetyNet Attestation](https://www.blackhat.com/docs/eu-17/materials/eu-17-Mulliner-Inside-Androids-SafetyNet-Attestation.pdf) _Black Hat Dev 17_
- [SafetyNet uptime updates](https://groups.google.com/forum/#!forum/safetynet-api-clients)
