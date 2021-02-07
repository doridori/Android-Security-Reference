# KeyStore

> KeyStore is responsible for maintaining cryptographic keys and their owners.
 
and 
 
> Use the Android Keystore provider to let an individual app store its own credentials that only the app itself can access. This provides a way for apps to manage credentials that are usable only by itself while providing the same security benefits that the KeyChain API provides for system-wide credentials. This method requires no user interaction to select the credentials.

## Why should I use it?

If you need to utilise and persist a cryptographic key in your application you either need to:

1. Request some input from the user (like a password) and either use this directly to derive a key or use it to encrypt a key which can then be persisted to disk
2. Store the key securely so it can't 'easily' be snooped

Some implementations may store a key inside shared prefs or raw on the filesystem, or worse, in the source code, which is a vulnerable approach to key management. 

KeyStore allows a way to store a key in hardware (if available) or in software (as an encrypted blob based off the device lock key) making it much harder to lift keys.

This is especially handy for any kind of challenge / response auth or other process where a compromise of the key itself would cause problems.


## Related API

- [`KeyStore`](http://developer.android.com/reference/java/security/KeyStore.html)
  - KeyStore is responsible for maintaining cryptographic keys and their owners.
- Used with   
  - [`Cipher`](http://developer.android.com/reference/javax/crypto/Cipher.html#init)
    - `Cipher.init(...)` takes a [`Key`](http://developer.android.com/reference/java/security/Key.html) which can represent a key that lives in the `KeyStore`
    - From **J-4.3-18** this can be the output of `KeyPair.getPrivate()`, where `KeyPair` is obtained from `KeyStore.getEntry(...)` e.g. RSA
    - From **M-6-23** this can be the output of `KeyStore.getKey(..)` if the `Key` for the passes `alias` is a `SecretKey` e.g. AES
    - `KeyStore` supported `Cipher` list is found on the [Android Keystore System](http://developer.android.com/training/articles/keystore.html) page
  - [`Key`](http://developer.android.com/reference/java/security/Key.html)
	    - Key is the common interface for all keys.  
  - Symmetric   
    - [`SecretKey`](http://developer.android.com/reference/javax/crypto/SecretKey.html) 
      - `KeyStore.getEntry(alias)` should return a `KeyStore.SecretKeyEntry` if `alias` represents an AES key
      - `KeyStore.getKey(alias)` should return a `SecretKey` if `alias` represents an AES key
    - [`SecretKeySpec`](https://developer.android.com/reference/javax/crypto/spec/SecretKeySpec.html)
      - A key specification for a SecretKey and also a secret key implementation that is provider-independent. It can be used for raw secret keys that can be specified as byte[].
    - [`KeyGenerator`](https://developer.android.com/reference/javax/crypto/KeyGenerator.html)
      - This class provides the public API for generating symmetric cryptographic keys. 
  - Asymmetric
	  - [`KeyPair`](http://developer.android.com/reference/java/security/KeyPair.html)
	    - KeyPair is a container for a public key and a private key. 
	    - `KeyStore.getEntry(..)` should return a `KeyStore.PrivateKeyEntry`
	  - [`KeyPairGenerator`](http://developer.android.com/reference/java/security/KeyPairGenerator.html)
	    - KeyPairGenerator is an engine class which is capable of generating a private key and its related public key utilizing the algorithm it was initialized with. 
	    - `KeyPairGenerator.getInstance(<cipherName>, "AndroidKeyStore");` for the key to be generated in the system keystore.
	  - [`KeyPairGeneratorSpec`](http://developer.android.com/reference/android/security/KeyPairGeneratorSpec.html)
	    - This provides the required parameters needed for initializing the KeyPairGenerator that works with Android `KeyStore`. The spec determines authorized uses of the key, such as whether user authentication is required for using the key, what operations are authorized, with what parameters, and the key's validity start and end dates
	    - **This class was deprecated in API level 23. Use `KeyGenParameterSpec` instead.**
  - Both
    - [`KeyGenParameterSpec`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
      - `AlgorithmParameterSpec` for initializing a `KeyPairGenerator` or a `KeyGenerator` of the Android Keystore system. 
      - **Since M-6-23**
      - More control over what the key can be used for and when
      - Supports authentication-required-before-use (lock screen | finger)

## Version changes

- **D-1.6-4**
  - Keystore implemented as a native keystore daemon that used a local socket as its IPC interface
- **J-4.3-18**
  - [Credential storage enhancements in Android 4.3](http://nelenkov.blogspot.co.uk/2013/08/credential-storage-enhancements-android-43.html)
  - Can store public / private keys via RSA `Cipher` support
  - `Signature` support for RSA variants
  - Can have multiple device users
    - Before 4.3 only PRIMARY user could use
  - CANT store AES keys
    - But could encrypt the AES key with created public / private key and in keystore
    - Can store directly via hidden apis - see link above
    - Can with [`Vault`](https://android.googlesource.com/platform/development/+/master/samples/Vault/src/com/example/android/vault/SecretKeyWrapper.java) wrapper class
      - Check out [g+ post](https://plus.google.com/+JeffSharkey/posts/9BmGb3xbPcA)
  - [Android 4.3 release notes](http://developer.android.com/about/versions/android-4.3.html#Security)
  - Daemon retired and replaced with binder interface
- **K-4.4-19**
  - Support for elliptic curve
  - `Signature` support for ECDSA variants
- **M-6-23**
  - First time for AES `Cipher` support (See ['Supported Algorithms'](http://developer.android.com/training/articles/keystore.html#SupportedAlgorithms)) 
  - `KeyGenerator` support for AES & HMAC
  - `KeyFactory` & `KeyPairGenerator` support for EC & RSA
  - `Mac` Hmac support
  - `SecretKeyFactory` support for AES & Hmac 
  - `Signature` increased support 
  - "Keys which do not require encryption at rest will no longer be deleted when secure lock screen is disabled or reset (for example, by the user or a Device Administrator). Keys which require encryption at rest will be deleted during these events.
  - [`KeyProtection`](http://developer.android.com/reference/android/security/keystore/KeyProtection.html)
    - Specification of how a key or key pair is secured when imported into the Android Keystore system.
  - `KeyStore` keys can require fingerprint auth before use 
    - "User authentication authorizes a specific cryptographic operation associated with one key. In this mode, each operation involving such a key must be individually authorized by the user". See `api/finger` for more.
- **N-7-24**
  - KeyMaster 2
  - [Key Attestation introduced](https://github.com/doridori/Android-Security-Reference/blob/master/framework/key_attestation.md)
- **O-8-26**
  - Keymaster 3
- **P-9-28**
  - Keymaster 4
  - [StrongBox](https://developer.android.com/training/articles/keystore#HardwareSecurityModule)
  - [Key importing](https://developer.android.com/training/articles/keystore#ImportingEncryptedKeys)
  - Support for 3DES encryption
  - Changes to version binding so that boot.img and system.img have separately set versions to allow for independent updates


## User Authenticating Key Use

Key authentication options are below, which the api and functionality offered differ pre and post M-23-6.

Keep in mind the different modes of usage will have have an impact of the stabilty/lifecycle of the key ([Android Security: The Forgetful Keystore](http://doridori.github.io/android-security-the-forgetful-keystore/)).

- No Auth
  - PreM
    - If [`.setEncryptionRequired()`](http://developer.android.com/reference/android/security/KeyPairGeneratorSpec.Builder.html#setEncryptionRequired()) is not set then any created keys will use NOT the keyguard input as input to the key blob KEK.
  - PostM
    - If [`.setUserAuthenticationRequired`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html#setUserAuthenticationRequired(boolean)) is `false` (default) then the keys are delt in the same way as PreM.
- Keyguard Authed
  - PreM
    - If [`.setEncryptionRequired()`](http://developer.android.com/reference/android/security/KeyPairGeneratorSpec.Builder.html#setEncryptionRequired()) is set then any created keys will use the keyguard input as input to the key blob KEK. 
  - PostM
    - If [`.setUserAuthenticationRequired`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html#setUserAuthenticationRequired(boolean)) is `true` and [setUserAuthenticationValidityDurationSeconds](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html#setUserAuthenticationValidityDurationSeconds(int)) > 0 then auth will be required via the keyguard (either via normal unlock or app requested keyguard unlock). 
- Finger Authed
  - PreM
    - N/A 
  - PostM
    - If [`.setUserAuthenticationRequired`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html#setUserAuthenticationRequired(boolean)) is `true` and [setUserAuthenticationValidityDurationSeconds](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html#setUserAuthenticationValidityDurationSeconds(int)) == -1 then finger auth will be required per use

## OS Auth

- OS access auth
  - Key access is tied to the apps UID
  - If rooted any user/app can in theory assume any UID
  - Access marshalled by keystore deamon on older apis and binder server on new ones 
 
### More on `.setUserAuthenticationRequired(true)` 

If a key has been created with `.setUserAuthenticationRequired(true)` then the user has to either authenticate via 

- [`KeyGuardManager.createConfirmDeviceCredentialIntent(...)`](https://developer.android.com/reference/android/app/KeyguardManager.html#createConfirmDeviceCredentialIntent) to prompt the user to enter pin, pattern or password. 
  - [ConfirmCredential](http://developer.android.com/samples/ConfirmCredential/index.html)
    - Uses `.setUserAuthenticationValidityDurationSeconds(AUTHENTICATION_DURATION_SECONDS)`  
    - This sample demonstrates how you can use device credentials (PIN, Pattern, Password) in your app to authenticate the user before they are trying to complete some actions
    - Will try to use key as part of crypto op after auth
- Fingerprint
  - Check [fingerprint.md](fingerprint.md) for more. Essentially needs to perform `KeyStore` op in fingerprint api auth callback.

## Losing Keys and how to handle

See [Android Security: The Forgetful Keystore](http://doridori.github.io/android-security-the-forgetful-keystore/) for losing Keys due to lock screen changes.

You can also lose Keys that require fingerprint auth when a new finger is enrolled. See [fingerprint.md](/api/fingerprint.md)

## Locking of Keystore

I have not encountered this myself but there are anecdontal reports of keystores becoming locked even when `setEncryptionRequired` == false. See [this SO post](http://stackoverflow.com/a/25790891/236743).

## Hardware vs Software & CTS

See [/hardware/keystore.md](/hardware/keystore.md)

## `KeyMaster` 

The hardware `KeyStore` is accessed through an [OEM specfific HAL](https://source.android.com/security/keystore/). There is a `softkeymaster` also. See some interesting comments and links around this [here](https://doridori.github.io/android-security-the-forgetful-keystore/#comment-3220919933).

See [android.googlesource.com](https://android.googlesource.com/platform/system/keymaster/+/master) for `keymaster` code. This [clone](https://github.com/geekboxzone/mmallow_system_keymaster/) maybe easier to navigate.


## CAs

CAs are also stored in the `KeyStore`. When added a custom CA device should prompt user to enter device credentials.


## Links

- [developer.android.com] [`KeyStore`](http://developer.android.com/reference/java/security/KeyStore.html) class
- [developer.android.com] [Android Keystore System](http://developer.android.com/training/articles/keystore.html)
  - Shows list of Ciphers (& more) supported by the `KeyStore` 
- Bugs / Vulns
  - [Android Security: The Forgetful Keystore](http://doridori.github.io/android-security-the-forgetful-keystore/) 
  - [Android keystore key leakage between security domains](http://jbp.io/2014/04/07/android-keystore-leak/)
    - Use `setEncryptionRequired()` to mitigate 
  - 30/6/2016 [Extracting Qualcomm's KeyMaster Keys – Breaking Android Full Disk Encryption](https://news.ycombinator.com/item?id=12007923) 
    - Talks about FDE but as KEK is lifted likely to impact `KeyStore` keys also 
- [Analysis of Secure Key Storage Solutions on Android](http://www.cs.kun.nl/~erikpoll/publications/AndroidSecureStorage.pdf)
- [Android KeyStore: what is the difference between “StrongBox” and “hardware-backed” keys?](https://proandroiddev.com/android-keystore-what-is-the-difference-between-strongbox-and-hardware-backed-keys-4c276ea78fd0)
- [How Secure is your Android Keystore Authentication ?](https://labs.mwrinfosecurity.com/blog/how-secure-is-your-android-keystore-authentication)
