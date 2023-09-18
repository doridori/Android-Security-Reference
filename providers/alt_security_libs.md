# Alt Security/Crypto Libs

The below are general purpose security libs which supply a subset of similar functionality to the Java providers

- [jedisct1/libsodium](https://github.com/jedisct1/libsodium)
  - C based 
  - _libsodium is not an OpenSSL replacement and doesn't want to be. It's a cryptographic primitive library and generally doesn't implement cryptographic protocols (eg. TLS, X.509, CMS, CRLs, OCSP_ [HN](https://news.ycombinator.com/item?id=12751886)
  - [lazysodium-android](https://github.com/terl/lazysodium-android)
- [google/tink](https://github.com/google/tink)
  - Tink is a cryptographic library that provides a safe, simple, agile and fast way to accomplish common cryptographic tasks. It is written by a group of cryptographers and security engineers at Google, but it is not an official Google product. In particular, is not meant as a replacement or successor of Keyczar.
  - Used by `androidx.security.crypto`
    - `+--- androidx.security:security-crypto:1.0.0 \--- com.google.crypto.tink:tink-android:1.5.0`
- [google/keyczar](https://github.com/google/keyczar)
  - Keyczar is an open source cryptographic toolkit designed to make it easier and safer for developers to use cryptography in their applications.
- [Secure Black Box](http://www.secureblackbox.com/)
  - SecureBlackbox is a collection of over 200 carefully crafted components and libraries that implement security standards and network communication protocols for every popular development platform (Windows, .NET, Linux, macOS, iOS, Java, Android)
