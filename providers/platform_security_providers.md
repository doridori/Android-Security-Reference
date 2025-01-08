# Platform Security Providers

Security functionality on Android is provided by various open src libs which vary depending on the OS version. There are native libs and java libs with JCE interfaces which coexist. 

## Quick Overview to terms

The crypto side of java has a few abbreviations associated with it, here are the most common ones:

- **JCA** [Java Cryptography Architecture](https://en.wikipedia.org/wiki/Java_Cryptography_Architecture)
  - The Java Cryptography Architecture (JCA) is a framework for working with cryptography using the Java programming language
  - The JCA uses a "provider"-based architecture (see below)
- **JCE** [Java Cryptography Extension](https://en.wikipedia.org/wiki/Java_Cryptography_Extension)
  - The Java Cryptography Extension (JCE) is an officially released Standard Extension to the Java Platform and part of Java Cryptography Architecture.  
  - "The basic difference between JCA and JCE is that JCE is an extension of JCA" [SO post](http://stackoverflow.com/a/32755954/236743)
  - The JCE framework places its classes in a different package, `javax.crypto`
  - Now they are both (JCA & JCE) shipped with Java SE and this division is not so obvious or important 
- **SPI** [Service Provider Interface](https://en.wikipedia.org/wiki/Service_provider_interface)
  - Service Provider Interface (SPI) is an API intended to be implemented or extended by a third party. It can be used to enable framework extension and replaceable components. 

Guide to terms like JCE & JSSE can be found [here](http://www.oracle.com/technetwork/java/javase/tech/index-jsp-136007.html).

## Native Implementations

#### OpenSSL

- Used since the beginning?
- OpenSSL version used - look in AOSP [openSSL version file](https://github.com/android/platform_external_openssl/blob/jb-release/openssl.version) for each platform tag 
  - K-4.3 `1.0.1c`
  - J-4.4 `1.0.1e`
  - L-5 `1.0.1h`
  - M-6 superceeded by BoringSSL 
- Backs `SecureRandom` since [4.2](http://android-developers.blogspot.co.uk/2013/02/using-cryptography-to-store-credentials.html)
- Compiling for Android [notes](https://scottyab.com/2017/05/how-to-build-and-integrate-openssl-into-your-android-ndk-project/)
	
#### BoringSSL

- Android's [BoringSSL repo](https://android.googlesource.com/platform/external/boringssl/) 
- Upstream [repo](https://boringssl.googlesource.com/boringssl/)	
  - Has build instructions 
- "BoringSSL is a fork of OpenSSL that is designed to meet Google’s needs."
  - First commit msg `BoringSSL versions Initial fork from f2d678e6e89b6508147086610e985d4e8416e867 (1.0.2 beta).`
  - Seems to pull in upstream forks patches and align to newer interfaces (1.1)
- Transition starting in 6.0

### AndroidKeyStore

Guessing this is for implementations provided/backed by the `KeyStore` API.

### Conscypt

[Official Docs](https://source.android.com/docs/core/ota/modular-system/conscrypt)

> "The Conscrypt module uses BoringSSL, a native library that is a Google fork of OpenSSL and which is used in many Google products for cryptography and TLS (most notably Google Chrome), in conjunction with Conscrypt code (both Java and native code)"

## Java JCE Providers

#### Apache Harmonys Crypto Provider
		
- Old and being phased out
- Limited JCE functionality

#### AndroidOpenSSL

- Introduced in 4 (SSL function only)
- Highest priority since 4.4
- Covers most of the same function as BC since 4.4
- Since 4.4 has been pulled out of core and can be used as a standalone lib (conscrypt)
- Backed by OpenSSL ( BoringSSL >= 6 ) natively
- Contained in the **Conscrypt** namespace (`com.android.org.conscrypt` on Android)
	- AndroidOpenSSL's [`Provider` def](https://android.googlesource.com/platform/external/conscrypt/+/android-n-preview-2/src/main/java/org/conscrypt/OpenSSLProvider.java#34) in `OpenSSLProvider` @ N preview v2
	  - Maps crypto def Strings to class definitions which conform to the SPI for that function 
	- AndroidOpenSSL's [SPI concrete class definitions](https://android.googlesource.com/platform/external/conscrypt/+/android-n-preview-2/src/main/java/org/conscrypt)  @ N preview v2
	  - Java bridge between the crypto SPI and the native [Open|Boring]SSL implementation  
	- [`NativeCrypto`](https://android.googlesource.com/platform/external/conscrypt/+/android-n-preview-2/src/main/java/org/conscrypt/NativeCrypto.java) JNI bridge 
	- [`NativeCryptoJni`](https://android.googlesource.com/platform/external/conscrypt/+/a6cef49/src/compat/java/org/conscrypt/NativeCryptoJni.java)
- Example flow through this JCE SPI implementation for a `Cipher` operation
  - Create a new [`Cipher`](https://android.googlesource.com/platform/libcore/+/d416195/luni/src/main/java/javax/crypto/Cipher.java#172) instance using `RSA/ECB/PKCS1Padding` 
  - This internally will grab the SPI implementation that can handle this "transformation"
  - [We can see](https://android.googlesource.com/platform/external/conscrypt/+/android-n-preview-2/src/main/java/org/conscrypt/OpenSSLProvider.java#212) the AndroidOpenSSL provider has registered to handle this with `OpenSSLCipherRSA$PKCS1`
  - [`OpenSSLCipherRSA`](https://android.googlesource.com/platform/external/conscrypt/+/android-n-preview-2/src/main/java/org/conscrypt/OpenSSLCipherRSA.java#46) conforms to the `CipherSpi` class, which [backs](https://android.googlesource.com/platform/libcore/+/d416195/luni/src/main/java/javax/crypto/Cipher.java#119) `Cipher`
  - Calling something like `Cipher.doFinal` [internally](https://android.googlesource.com/platform/external/conscrypt/+/android-n-preview-2/src/main/java/org/conscrypt/OpenSSLCipherRSA.java#265) calls `NativeCrypto.RSA_private_encrypt` which is a [JNI method](https://android.googlesource.com/platform/external/conscrypt/+/android-n-preview-2/src/main/java/org/conscrypt/NativeCrypto.java#121)
  - This calls through a loaded `.so` lib (not included in AOSP or BoringSSL repos afaik) to [rsa.c](https://boringssl.googlesource.com/boringssl/+/master/crypto/rsa/rsa.c#222). This internally calls through to the loaded `struct RSA` (`rsa->meth->decrypt`) and will perform the op based upon the structs config. 
- Was part of `libcore` until 4.4 

#### BouncyCastle

- Android used an old version of BC ("[crippled]")
- Was the default security Java Security API provider
- Package was renamed at 3.0. Before this issues if including a newer version of BC (hence [SC](https://rtyley.github.io/spongycastle/))
- Offered full JCE functionality
- Unsure how much (if any) functionality delegated to native (i.e. [Open|Boring]Ssl) implementation. Should look at the CSPRNG...

#### AndroidKeyStoreBCWorkaroundProvider

[Check out the code comments](https://android.googlesource.com/platform/frameworks/base/+/a8c837f/keystore/java/android/security/keystore/AndroidKeyStoreBCWorkaroundProvider.java)

#### SpongyCastle

- Many people import this so can use latest BC, pretty much a jarjarlinks rename so no namespace clashes with BC. 
- If going to use best not to make default but still set staically so can use with direct sepcifier throughout app

#### Crypto

- Small (buggy) provider mostly offering SHA1 based operations (plus DSA on older platform versions). Reduced to SHA1PRNG and then removed completely in N.
- [Security "Crypto" provider deprecated in Android N](http://android-developers.blogspot.co.uk/2016/06/security-crypto-provider-deprecated-in.html?utm_source=androiddevdigest)

#### CertPathProvider

[Src](https://android.googlesource.com/platform/libcore/+/bf04039/ojluni/src/main/java/sun/security/provider/CertPathProvider.java)

## Updating platform Provider from your app

See [Updating Your Security Provider to Protect Against SSL Exploits](http://developer.android.com/training/articles/security-gms-provider.html). Not clear if updates `BoringSSL` or just the Java `AndroidOpenSSL` SPI (or both). 

## What algorithms do the providers supply?

- [What crypto algorithms does Android support?](http://stackoverflow.com/questions/7560974/what-crypto-algorithms-does-android-support)
- [Android support for Elliptic Curves (EC) in KeyPairGenerator](https://www.danielhugenroth.com/posts/2021_07_ec_curves_on_android/)
