# Version 1 signing

> v1 signatures do not protect some parts of the APK, such as ZIP metadata. The APK verifier needs to process lots of untrusted (not yet verified) data structures and then discard data not covered by the signatures. This offers a sizeable attack surface. Moreover, the APK verifier must uncompress all compressed entries, consuming more time and memory. To address these issues, Android 7.0 introduced APK Signature Scheme v2.

From [source.android.com/security/apksigning/](https://source.android.com/security/apksigning/)

This is the same as standard Java's `jarsigner`. Explanation [here](https://web.pa.msu.edu/reference/jdk-1.2.2-docs/tooldocs/win32/jarsigner.html). 

# Version 2 signing

> During validation, v2 scheme treats the APK file as a blob and performs signature checking across the entire file. Any modification to the APK, including ZIP metadata modifications, invalidates the APK signature. This form of APK verification is substantially faster and enables detection of more classes of unauthorized modifications.

From [source.android.com/security/apksigning/](https://source.android.com/security/apksigning/#v2)

# Version 3

[V3](https://source.android.com/security/apksigning/v3)

- Supports key rotation
- Android 9+

# Signing with multiple certs

- Seems not very well supported anecdotally
- [Android code signing](https://nelenkov.blogspot.co.uk/2013/04/android-code-signing.html)
  - Multiple signatures can be performed, resulting in multiple .SF and .RSA/DSA/EC files in the JAR file's META-INF/ directory.
  
# Security Metadata

- [Google Play security metadata and offline app distribution](https://android-developers.googleblog.com/2018/06/google-play-security-metadata-and.html)

# KeyTool

[Oracle docs](https://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html)

## Signing key strength

- [Guardian Project] [How to Migrate Your Android App’s Signing Key](https://guardianproject.info/2015/12/29/how-to-migrate-your-android-apps-signing-key/)
- Signiture algorithm _defaults_ depend on the `-keyalg` and `-keysize`
- `keytool` man _"The signature algorithm is derived from the algorithm of the  underlying  private  key:  If the underlying private key is of type "DSA", the default signature algorithm is "SHA1withDSA", and if the underlying private key is of type "RSA", the default signature algorithm is "MD5withRSA"."_
- `keytool` defaults change depending on JDK/Java version

```
In generating a public/private key pair, the signature algorithm (-sigalg option) is derived from the algorithm of the underlying private key:

If the underlying private key is of type "DSA", the -sigalg option defaults to "SHA1withDSA"
If the underlying private key is of type "RSA", the -sigalg option defaults to "SHA256withRSA".
If the underlying private key is of type "EC", the -sigalg option defaults to "SHA256withECDSA".
```

From http://docs.oracle.com/javase/7/docs/technotes/tools/solaris/keytool.html

Takeaway: _Always be specific!_

# Inspecting signing info

## Via APK

[https://stackoverflow.com/a/11331951/236743](https://stackoverflow.com/a/11331951/236743)

## On Android

- [guardianproject/checkey](https://github.com/guardianproject/checkey)
  - Releated to interesting F-Droid [reproducable builds](https://f-droid.org/wiki/page/Deterministic,_Reproducible_Builds) project
    - [blog post](https://guardianproject.info/2015/02/11/complete-reproducible-app-distribution-achieved/)

## Via Gradle (Android)

`android -> signingReport` gradle task

## Via `term`

```
$ keytool -list -v -keystore ./signing/debug.keystore -alias androidDebugKey
Enter keystore password: <android>
Alias name: key_alias
Creation date: 19-May-2017
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: O=ME
Issuer: O=ME
Serial number: 3675053
Valid from: Thu May 18 14:02:08 BST 2017 until: Mon May 12 14:02:08 BST 2042
Certificate fingerprints:
	 MD5:  DE:EA:4B:B6:8B:60:68:4B:9F:A8:BA:E5:1C:6D:00:02
	 SHA1: 7F:BA:DE:19:87:1F:31:E5:82:59:E2:98:D3:04:C9:C5:70:B3:00:02
	 SHA256: F5:57:F3:91:12:3E:12:AA:ED:C8:7B:C3:1C:53:9A:63:BB:11:F2:63:C1:F7:F1:29:7B:3F:56:14:C1:50:02:12
	 Signature algorithm name: SHA256withRSA
	 Version: 3
```

# Checking from in app

- [Checking own signing cert](https://stackoverflow.com/questions/9293019/get-certificate-fingerprint-from-android-app/22506133#22506133)

# Assorted Reading

- [Android APK signature scheme v3: context and new opportunities](https://www.guardsquare.com/en/blog/android-apk-signature-scheme-v3-context-and-new-opportunities)
  - Dexguard tooling supported v3 signing before Android tooling!
