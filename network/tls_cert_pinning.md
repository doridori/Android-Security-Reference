# TLS Cert Pinning

There are various approaches to cert pinning on android. What they all have in common is that they validate the cert chain in use by looking for compile time known certs. Below is a TLDR of a simple approach as spoken about in the below links.

- Depending on the approach these can be Root, Intermediary, or Leaf (or a combo of the above) depending on requirements. 
- Pin the SHA256 of Subject Public Key Info ([SPKI](https://tools.ietf.org/html/rfc5280#section-4.1.2.7))
- _"...developers should not check pins against the list of certificates sent by the server. Instead, pins should be checked against the new, 'clean' chain that is created during SSL validation."_. See Blog Posts below for more info, **lots of pinning implementations in the wild are broken**!
- Easiest by far is to use Okhttp's `CertificatePinner` which accounts for ["cleaning the chain" (3.2.0)](https://github.com/square/okhttp/blob/parent-3.2.0/okhttp/src/main/java/okhttp3/CertificatePinner.java#L149). See [CertificatePinner](https://square.github.io/okhttp/3.x/okhttp/okhttp3/CertificatePinner.html) for more details.

# Changes in Nougat

Can pin via xml as shown 

- [developer.android.com] [Network Security Configuration](http://developer.android.com/preview/features/security-config.html#CertificatePinning).
- [android-developers.blogspot] [Changes to Trusted Certificate Authorities in Android Nougat](http://android-developers.blogspot.co.uk/2016/07/changes-to-trusted-certificate.html) for more. 

# Links

- Blog posts
  - [Paypal Engineering - Key Pinning in Mobile Applications](https://www.paypal-engineering.com/2015/10/14/key-pinning-in-mobile-applications/)
  - [Testing for CVE-2016-2402 and similar pinning issues](https://koz.io/pinning-cve-2016-2402/)
    - Talks about [An Examination of Ineffective Certificate Pinning Implementations](https://www.cigital.com/blog/ineffective-certificate-pinning-implementations/) 
  - [Okhttp pinning discussion](https://github.com/square/okhttp/issues/173) 
- Patching out
  - [Bypassing SSL Pinning in Android Applications](https://serializethoughts.com/2016/08/18/bypassing-ssl-pinning-in-android-applications/)
  - [Android-SSL-TrustKiller](https://github.com/iSECPartners/Android-SSL-TrustKiller/blob/master/src/com/android/SSLTrustKiller/Hook.java)
- RFCs
  - [rfc7469 `Public Key Pinning Extension for HTTP`](https://tools.ietf.org/html/rfc7469) (HPKP TOFU pin)
  - [rfc5280 Internet X.509 Public Key Infrastructure Certificate and Certificate Revocation List (CRL) Profile](https://tools.ietf.org/html/rfc5280)
- Guides  
  - [OWASP Public Key Pinning](https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning)
    - Has Android examples
- OpenSrc 
  - [moxie0/AndroidPinning](https://github.com/moxie0/AndroidPinning)
    - Has manual inclusion of system TrustManager 
  - [iSECPartners/android-ssl-bypass](https://github.com/iSECPartners/android-ssl-bypass)
  - [TrustKit-Android](https://github.com/datatheorem/TrustKit-Android)
- Talks
  - [Pinning: Not as simple as it sounds](https://usmile.at/symposium/2017/program/kozyrakis)

# Random Notes

- When creating a `TrustManagerFactory` pass in null to get system `TrustManager`. Can pin while extending system TrustManager. 
