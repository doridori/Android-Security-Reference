

# Link Collections

- [tanprathan/MobileApp-Pentest-Cheatsheet](https://github.com/tanprathan/MobileApp-Pentest-Cheatsheet#security-libraries)

# Analysis

## Both Static & Dynamic Analysis Tools

- [ajinabraham/Mobile-Security-Framework-MobSF](https://github.com/ajinabraham/Mobile-Security-Framework-MobSF)

## Automated Static Analysis Tools

- [OWASP list](https://www.owasp.org/index.php/Static_Code_Analysis)
- [QARK](https://github.com/linkedin/qark)
  - Nice apk static analysis tool for Android
  - No way to supress certain warnings / errors so probably would not want to run as part of CI build
- [bherbst/OpenSSL-Checker](https://github.com/bherbst/OpenSSL-Checker)
  - A Gradle plugin for checking whether an .apk or an .aar contains OpenSSL versions with known vulnerabilities
- Veracode
  - Commercial
- CheckMarx
  - Commercial 
- [jeremylong/DependencyCheck](https://github.com/jeremylong/DependencyCheck)
  - OWASP dependency-check is a software composition analysis utility that detects publicly disclosed vulnerabilities in application dependencies
- [NationalSecurityAgency/ghidra](https://github.com/NationalSecurityAgency/ghidra)
  - Note: [Ghidra opens up JDWP in debug mode listening on port 18001, you can use it to execute code remotely](https://twitter.com/hackerfantastic/status/1103087869063704576)
- [GuardSqaure AppSweep](https://www.guardsquare.com/blog/proguard-playground-beta-graduation)
  

### VMs

- [DroidScope](https://www.usenix.org/conference/usenixsecurity12/technical-sessions/presentation/yan)
  - DroidScope is a malware analysis engine based on QEMU. It adds instrumentation on several levels, https://www.usenix.org/conference/usenixsecurity12/technical-sessions/presentation/yanmaking it possible to fully reconstruct the semantics on the hardware, Linux and Java level  
- [PANDA](https://github.com/moyix/panda) 

### Hex

- [Veles](https://codisec.com/veles/)
- [https://twitter.com/i/moments/841916822014332930](https://twitter.com/i/moments/841916822014332930)

## Dynamic Analysis

- [rednaga/native-shim](https://github.com/rednaga/native-shim)
  - NDK / JNI 
- [FЯIDA](http://www.frida.re/)
  - Inject JavaScript to explore native apps on Windows, Mac, Linux, iOS and Android. 
  - Can do system call analysis
  - [Using Frida on Android without root](https://koz.io/using-frida-on-android-without-root/)
  - [sensepost/objection](https://github.com/sensepost/objection)
    - runtime mobile exploration
  - [dweinstein/awesome-frida](https://libraries.io/github/dweinstein/awesome-frida)
  
### Network traffic

- [Nogotofail](https://github.com/google/nogotofail) 
  - Nogotofail is a network security testing tool designed to help developers and security researchers spot and fix weak TLS/SSL connections and sensitive cleartext traffic on devices and applications in a flexible, scalable, powerful way. 
  
### MemDump

- [Introduction to Fridump](http://pentestcorner.com/introduction-to-fridump/)
  
# Decompilers

## Mixed (Android & Java)

- [Decompilers online](http://www.javadecompilers.com/)
  - Online Android & Java decompilation
- [Konloch/bytecode-viewer](https://github.com/Konloch/bytecode-viewer)
  - A Java 8 Jar & Android APK Reverse Engineering Suite (Decompiler, Editor, Debugger & More)
  - Seems broken for APKs build with AS 3.0 and AGP 3.0 (if not before). Project may be abandoned, lots of issues and no activity in 10 months _Nov 17_

## Java

- [CFR](http://www.benf.org/other/cfr/)
  - CFR will decompile modern Java features - Java 8 lambdas (pre and post Java beta 103 changes), Java 7 String switches etc, but is written entirely in Java 6. (FAQ)
  - Cmd line, simple; 
  - **Currently my goto for `jar->src`**
- [JDGui](http://jd.benow.ca/)
  - The “Java Decompiler project” aims to develop tools in order to decompile and analyze Java 5 “byte code” and the later versions.
  - Can use to export source from jars
  - GUI; I always find to be a bit clunky and broken (export src / restricted search) but others seem to like it
- `javap`!
  - JDK `class` to bytecode
  - [Exploring Java bytecode](https://blog.nishtahir.com/2015/09/12/exploring-java-byte-code/)
  - [Keynote: Sinking Your Teeth Into Bytecode](https://skillsmatter.com/skillscasts/10012-keynote-sinking-your-teeth-into-bytecode)

Super-pro `javap` script...

```
#!/bin/bash
for filename in *.class; do
  echo $filename
  javap $filename > out/"$filename".out
done

```

### Android

- [google/android-classyshark](https://github.com/google/android-classyshark)
  - Quick view for apk package / class breakdown  
- [Deguard](http://www.apk-deguard.com/) 
  - Statistical Deobfuscation for Android
- [APKtool](https://ibotpeaches.github.io/Apktool/) 
  - 12.5k github stars and maintained  
  - A tool for reverse engineering Android apk files
  - jar based
- [google/Enjarify](https://github.com/google/enjarify)
  - Ment to improve on Dex2Jar in terms of what it can handle
  - Python3 based
  - **Currently my goto for `dex->jar`**
- [Dex2Jar](https://github.com/pxb1988/dex2jar)
  - Pretty reliable
  - Can view after in something like JD-GUI
- [sonyxperiadev/ApkAnalyser](https://github.com/sonyxperiadev/ApkAnalyser)
  - ApkAnalyser is a static, virtual analysis tool for examining and validating the development work of your Android app.
- [Android Studios ApkAnalyser](https://developer.android.com/studio/build/apk-analyzer.html)
- [CalebFenton/apkfile](https://github.com/CalebFenton/apkfile)
  - Android app analysis and feature extraction library
- [skylot/jadx](https://github.com/skylot/jadx)
  - 27k github stars and maintained  
  - Dex to Java decompiler
  - Drag and drop apk decompilation
  - Nice search

## Native

- [IDA Pro](https://www.hex-rays.com/products/ida/)
  - ARM, MIPS, ELF, Java Bytecode (-> SMALI)
  - Combines an interactive, programmable, multi-processor disassembler coupled to a local and remote debugger and augmented by a complete plugin programming environment 
- [JEB](https://www.pnfsoftware.com/)
  - Commercial Decompiler (& Debugger)
- [Radare2 ](http://rada.re/r/)
  - Radare is a portable reversing framework
- [Hopper](https://www.hopperapp.com/)
  - The OS X and Linux Disassembler

# Pen|Security Testing Frameworks / Collections

- [Drozer](https://labs.mwrinfosecurity.com/tools/drozer/)
  - drozer is a comprehensive security audit and attack framework for Android
  - Needs to execute on a device the apk is installed on
- NowSecure
  - [Lab Suite](https://www.nowsecure.com/lab/) _Commercial_
- [MonkOp](http://www.monkop.com/index.html)
  - Upload apk to remote 
- [SUPER Analyzer](http://superanalyzer.rocks/): [Source](https://github.com/SUPERAndroidAnalyzer/super/) : Secure, Unified, Powerful and Extensible Rust Android Analyzer 

## Walkthroughs

- [Hacking mobile login tokens tricky but doable, says reverse-engineer](http://www.theregister.co.uk/2016/09/02/mobile_2fa_shortcomings/)
  - Links to _very_ comprehensive [paper](https://regmedia.co.uk/2016/09/02/hacking_soft_tokens_-_bernhard_mueller.pdf) about analysing and closing OTP banking apps 

# Linux Tools

## Guides

- [Linux Debugging Tools You'll ♥](http://jvns.ca/debugging-zine.pdf)

## Process Analysis

- `strace`
  - Strace is a standard Linux utility that is used to monitor interaction between processes and the kernel
- `ftrace` 
  - On a rooted device, ftrace can be used to trace kernel system calls in a more transparent way than is possible with strace 

## File System Analysis

_Below are taken from [FileSystem Monitor Tool For iOS and Android](https://www.nowsecure.com/blog/2016/02/18/filesystem-monitor-tool-for-ios-and-android/?mkt_tok=3RkMMJWWfF9wsRokv6%2FIZKXonjHpfsX56uovWaCylMI%2F0ER3fOvrPUfGjI4DTsBnI%2BSLDwEYGJlv6SgFSLDEMbhlzbgFXBI%3D)_

- `inotify` - In Linux, and therefore Android, the inotify syscall provides access to receive the filesystem events happening on a specific file or directory. 
- `fanotify` - Improved `inotify` added in Android 5.0
- [fsmon](https://github.com/nowsecure/fsmon)
- [Linux debugging tools I love](http://jvns.ca/blog/2016/07/03/debugging-tools-i-love/)

# Device Vuln Testing

- [X-Ray](https://labs.duo.com/xray/)
  - Attempts exploits on device 
- [NowSecure Android VTS](https://github.com/AndroidVTS/android-vts)

# OS Bundles

- [Kali Linux](https://www.kali.org/)
  - Kali Linux is a Debian-based Linux distribution aimed at advanced Penetration Testing and Security Auditing. Kali contains several hundred tools aimed at various information security tasks, such as  Penetration Testing, Forensics and Reverse Engineering. 
- [Santoku](https://santoku-linux.com/)
- [AndroidTamer](https://androidtamer.com) : Android Tamer is a Virtual / Live Platform for Android Security professionals. This Environment allows people to work on large array of android security related task’s ranging from Malware Analysis, Penetration Testing and Reverse Engineering.
- [MobiSecLive](https://sourceforge.net/projects/mobisec/?source=navbar) : This project was a DARPA CFT funded project that is now being released through OWASP. It is focused on providing a live environment for mobile security testing, forensics, reverse engineering and wireless analysis.

