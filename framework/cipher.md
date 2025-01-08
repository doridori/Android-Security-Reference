# Cipher

For using `Cipher` with the `KeyStore` see [/api/keystore.md](/api/keystore.md)

## Beware of the default IV!

(see [post](https://doridori.github.io/Android-Security-Beware-of-the-default-IV/))

Its easy to accidentally use the [sometimes](https://stackoverflow.com/questions/31036780/android-cryptography-api-not-generating-safe-iv-for-aes) default zeroed out IV when using `Cipher`, which can cause vulns as some encryption modes rely on an IV being set. 

For example, if using `AES/CBC/*` you **[must](http://security.stackexchange.com/questions/35210/encrypting-using-aes-256-do-i-need-iv/35216#35216)** set an IV! This can be done with 

```java
byte[] iv = //gen random iv
c.init(Cipher.ENCRYPT_MODE, keySpec, new IvParameterSpec(iv));
```

## Software supported algorithms

- [SO - What crypto algorithms does Android support?](http://stackoverflow.com/questions/7560974/what-crypto-algorithms-does-android-support)
- [Android support for Elliptic Curves (EC) in KeyPairGenerator](https://www.danielhugenroth.com/posts/2021_07_ec_curves_on_android/)
