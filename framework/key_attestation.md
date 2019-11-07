# Gives

- Passed verified boot
- has recent security patch
- key is protected by TEE or StrongBox
- Key requires user auth in X seconds
- Firmware hash (compatible P+ devices)

# Hardware or software backed

- All 7 implementations are software backed
- 8 is optional

The [Auditor](https://play.google.com/store/apps/details?id=app.attestation.auditor&hl=en_GB) project's Google play listed gives a hint at the devices which are supporting hardware key attestation. 

# OS version changes

- **N-7-24**
  - Keymaster 2
  - [Key Attestation](https://developer.android.com/training/articles/security-key-attestation.html#certificate_schema)
    - Can prove to 3rd partys that a hardware keystore exists with certain keys by signing a representation using a factory supplied key 
    - KeyStore now returns a chain for a key alias which can be used to verify that the device has passed CTS testing
    - All devices shipping with 7 had software attestation only
  - Hardware KeyStore manditory [IO link](https://youtu.be/XZzLjllizYs?t=571) 
  - "When the device implementation supports a secure lock screen it MUST back up the keystore implementation with secure hardware" [ACD](http://source.android.com/compatibility/7.0/android-7.0-cdd.html#9_11_keys_and_credentials)
  - "Note that if a device implementation is already launched on an earlier Android version, and does not have a fingerprint scanner, such a device is exempted from the requirement to have a hardware-backed keystore." also from keys and creds
  - [Version Binding](https://source.android.com/security/keystore/version-binding)
- **O-8-26**
  - Keymaster 3 support
  - All O+ devices have attestation API
  - If shipped with 0 the attestation API MUST be hardware backed ([CDD](https://source.android.com/compatibility/8.0/android-8.0-cdd#9_11_keys_and_credentials))
  - [Includes ID attestation](https://source.android.com/security/keystore/attestation#id-attestation)
- **P-9-28**
  - Keymaster 4
  - Support for embedded Secure Elements
  - Support for secure key import
  - Support for 3DES encryption
  - Changes to version binding so that boot.img and system.img have separately set versions to allow for independent updates

# Attestation Signing

## Official docs

From [training/articles/security-key-attestation](https://developer.android.com/training/articles/security-key-attestation#attestation-v3)

> Note: Before you verify the properties of a device's hardware-backed keys in a production-level environment, you should make sure that the device supports hardware-level key attestation. To do so, you should check that the attestation certificate chain contains a root certificate that is signed with the Google attestation root key and that the attestationSecurityLevel element within the key description data structure is set to the TrustedEnvironment security level.

> In addition, it's important to verify the signatures in the certificate chain and to check that none of the keys in the chain have been revoked by checking the Certificate Revocation List endpoint listed in each certificate. Unless all are valid and the root is the Google root key mentioned above, you shouldn't fully trust the attestation. Note, however, that devices containing revoked certificates are still at least as trustworthy as devices that only support software attestation. Having a fully-valid attestation is a strong positive indicator. Not having one is a neutral—not negative—indicator.

and

> If the device supports hardware-level key attestation, the root certificate within this chain is signed using an attestation root key, which the device manufacturer injects into the device's hardware-backed keystore at the factory.

> Note: On devices that ship with hardware-level key attestation, Android 7.0 (API level 24) or higher, and Google Play services, the root certificate is signed with the Google attestation root key. You should verify that this root certificate appears within Google's list of root certificates.

## Official bootcamp

From [here](https://source.android.com/security/reports/Android-Bootcamp-2016-Android-Keystore-Attestation.pdf)

> Key Provisioning 
> ● Google will certify attestation keys for Google-approved devices.
> ● Keys will be deployed to device batches: min 10K devices per key.
> ● Initially, Google will create the keys as well as certify them.
> ● The process will be very similar to the Widevine key distribution process (will likely use the same delivery method).

## Official source

[source.android.com/security/keystore/attestation](https://source.android.com/security/keystore/attestation)

## Official guides

[google/android-key-attestation](https://github.com/google/android-key-attestation/blob/master/server/src/main/java/com/android/example/KeyAttestationExample.java)

## Google root certs

Guessing the root certs above, which would need to be validated, are found here https://pki.goog/.

## Example Attestations

[https://twitter.com/DanielMicay/status/1029831765211271168](https://twitter.com/DanielMicay/status/1029831765211271168)
