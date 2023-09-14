_See [/framework/keystore.md](/framework/keystore.md) for API and usage information._

# Hardware backed

Some devices have hardware backed keystore storage, which was introduced in 4.3 but not mandatory (Nexus devices since Nexus 4 have had this afaik). 

If a [CTS](http://static.googleusercontent.com/media/source.android.com/en//compatibility/android-cdd.pdf) compatible fingerprint functionality is available then

## TEE

> MUST have a hardware-backed keystore implementation, and perform the fingerprint matching in a Trusted Execution Environment (TEE) or on a chip with a secure channel to the TEE.

See the [hardware/TEE.md](/hardware/TEE.md) for more inc. vulns.

_A [TEE](https://en.wikipedia.org/wiki/Trusted_execution_environment) (or [TPM](https://en.wikipedia.org/wiki/Trusted_Platform_Module)) can change a `KeyStore` attack from **offline** (which would be against a software KeyStores data) to **online** (running on the device issuing commands to the hardware). This further prevents extraction of the private key and potentially throttling of access attempts (and therefore potential exploitation). See Androids [Trusty TEE](https://source.android.com/security/trusty/index.html)_

## Strongbox

Strongbox is a keymaster implemented in tamper-resitant hardware (e.g. an HSM &| SE). This can be present in [addition](https://source.android.com/security/best-practices/hardware) to the TEE based keymaster implementation.

Strongbox has a [subset](https://developer.android.com/training/articles/keystore.html#HardwareSecurityModule) of keys types available for use.

## CDD/CTS

The CDD history of the `KeyStore` is quite interesting. There are a few fundamental features for which ROM inclusion or hardware presense of will make some requirements around `KeyStore` implementation come into scope. These are around:

- Secure Lock Screens (can check with `isKeyguardSecure()`)
- Fingerprint
- Disk Encryption

And also some clauses only seem to come into effect if a device _ships_ with an OS version as opposed to _is upgraded_ to an OS version.

### CDD M-6.0-23

As of M-6-23 hardware backed `KeyStore` is not a requirement, as indicated at the bottom of the [9.11. Keys and Credentials](https://source.android.com/compatibility/6.0/android-6.0-cdd#9_11_keys_and_credentials):

> Note that while the above TEE-related requirements are stated as STRONGLY RECOMMENDED, the
Compatibility Definition for the next API version is planned to changed these to REQIUIRED. If a
device implementation is already launched on an earlier Android version and has not implemented a
trusted operating system on the secure hardware, such a device might not be able to meet the
requirements through a system software update and thus is STRONGLY RECOMMENDED to
implement a TEE.


### CDD N-7.0-24

Hardware based `KeyStore` is [now mandatory in for devices release with N](https://youtu.be/XZzLjllizYs?t=571); In the updated CDD [9.11. Keys and Credentials](https://source.android.com/compatibility/7.0/android-7.0-cdd#9_11_keys_and_credentials).

However it does state

> Note that if a device implementation is already launched on an earlier Android version, such a device is exempted from the requirement to have a hardware-backed keystore, unless it declares the android.hardware.fingerprint feature which requires a hardware-backed keystore.

I have observerd on a 6P. which does support fingerprint and running 8.1.0 that it supports RSA in hardware but not AES.

### CDD N-7.1-25

An Interesting change to section `9.11` in `7.1` is that:

> MUST have implementations of RSA, AES, ECDSA and HMAC cryptographic algorithms and MD5, SHA1, and SHA-2 family hash functions to properly support the Android Keystore system's supported algorithms in an area that is securely isolated from the code running on the kernel and above. Secure isolation MUST block all potential mechanisms by which kernel or userspace code might access the internal state of the isolated environment, including DMA. The upstream Android Open Source Project (AOSP) meets this requirement by using the Trusty implementation, but another ARM TrustZone-based solution or a third-party reviewed secure implementation of a proper hypervisor-based isolation are alternative options.

Previous to this change, _harware keystore support_ was pretty ambiguous. A device could have a hardware `KeyStore`, but not support all the [Supported Algorithms](https://developer.android.com/training/articles/keystore.html#SupportedAlgorithms) _in hardware_. This can be seen on the Nexus 5 running M-6-23, which supports hardware RSA but not AES; which would result in the AES operation being performed in the _software_ `KeyStore`.

### CDD S-12-31

No change to section `9.11` from the CDD for N-7.1-25 above.

## API checking

The presence of hardware backed key storage can be checked via the [`KeyChain.isBoundKeyAlgorithm`](http://developer.android.com/reference/android/security/KeyChain.html#isBoundKeyAlgorithm(java.lang.String)) method and for M-6-23+ [`KeyInfo.isInsideSecureHardware ()`](http://developer.android.com/reference/android/security/keystore/KeyInfo.html#isInsideSecureHardware()). See below

> Android also now supports hardware-backed storage for your KeyChain credentials, providing more security by making the keys unavailable for extraction. That is, once keys are in a hardware-backed key store (Secure Element, TPM, or TrustZone), they can be used for cryptographic operations but the private key material cannot be exported. Even the OS kernel cannot access this key material. While not all Android-powered devices support storage on hardware, you can check at runtime if hardware-backed storage is available by calling KeyChain.IsBoundKeyAlgorithm().

This means on a hardware backed device even if rooted the private keys cannot be extracted! They can still be used locally however (by an attacker).

> Key material may be bound to the secure hardware (e.g., Trusted Execution Environment (TEE), Secure Element (SE)) of the Android device. When this feature is enabled for a key, its key material is never exposed outside of secure hardware. If the Android OS is compromised or an attacker can read the device's internal storage, the attacker may be able to use any app's Android Keystore keys on the Android device, but not extract them from the device. This feature is enabled only if the device's secure hardware supports the particular combination of key algorithm, block modes, padding schemes, and digests with which the key is authorized to be used. To check whether the feature is enabled for a key, obtain a KeyInfo for the key and inspect the return value of KeyInfo.isInsideSecureHardware().

and 

> If the Android OS is compromised or an attacker can read the device's internal storage, the attacker may be able to use any app's Android Keystore keys on the Android device, but not extract them from the device

The software blob can however be read on rooted devices. See [nelenkov/keystore-decryptor](https://github.com/nelenkov/keystore-decryptor).

## Links

- [Nikolay Elenkov - Keystore redesign in Android M](https://nelenkov.blogspot.co.uk/2015/06/keystore-redesign-in-android-m.html)
