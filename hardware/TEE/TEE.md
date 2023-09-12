# TEE

## General Info

- See [Trusty TEE](https://source.android.com/security/trusty/index.html)
- See [Mobile Platform Security: Trusted Execution Environments](http://asokan.org/asokan/Padova2014/tutorial-mobileplatsec.pdf)

From 'Android Security Internals' the Nexus 4 was TrustZone enabled, with QSEE implemented on top of that. Most likely uses a single master key-encryption key (KEK) which protects all user-generated keys but implementations may use KEK per key and / or hardware fused KEKs.

## Chip Security Foundations

- [ARM TrustZone](https://www.arm.com/products/security-on-arm/trustzone)
  - TrustZone is hardware-based security built into SoCs by semiconductor chip designers who want to provide secure end points and a device root of trust. 
  - TrustZone technology provides a foundation for system-wide security and the creation of a trusted platform. Any part of the system can be designed to be part of the secure world, including debug, peripherals, interrupts and memory.
  - TrustZone technology within Cortex-A based application processors is commonly used to run trusted boot and a trusted OS to create a Trusted Execution Environment (TEE)
    - [Cortex-A](https://www.arm.com/products/processors/cortex-a) processors power many mobile devices. For example, the Pixel phone uses a [Kryo](https://en.wikipedia.org/wiki/Kryo_(microarchitecture)) microarchitecture/CPU (as part of the Snapdragon 821 processor SoC) using the ARMv8-A Instruction Set. 
- x86 //TODO
  
## TEEs

- [What is a TEE?](https://www.globalplatform.org/mediaguidetee.asp)

### Built on top of TrustZone
   
- Qualcomm's Secure Execution Environment (`QSEE`)
  - Found on Snapdragon SoCs
  - [Exploring Qualcomm's Secure Execution Environment](http://bits-please.blogspot.co.uk/2016/04/exploring-qualcomms-secure-execution.html)
  - Interacts with the TrustZone Kernel via `qseecom` kernel driver
- [Trusty TEE](https://source.android.com/security/trusty/)
  - AOSP TEE
  - Any TEE OS (not just Trusty) can be used for TEE implementations
  - Currently all Trusty applications are developed by a single party and packaged with the Trusty kernel image.
  - NOT GlobalPlatform conforming (this may have changed)
  - [Ships on the Pixel 6 as part of Tensor](https://blog.google/products/pixel/introducing-google-tensor/)
- Giesecke & Devrient (G&D) `MobiCore`
  - [Looks like](http://www.smartinsights.net/Secure-Transactions-News/ARM-Gemalto-and-G-D-launch-Trustonic-for-TEE) it used to be Qualcomms TEE
  - [G&D CARTES 2012 Demo presentation](https://www.gi-de.com/gd_media/media/documents/complementary_material/events_1/04_STE_CARTES__Demo_Presentation.pdf)
  - **Samung** have used in the past (and may still)
- Trustonic TEE (`Kinibi` _prev. Mobicore?_)
  - [Set up by ARM, Gemalto, G&D](http://www.smartinsights.net/Secure-Transactions-News/ARM-Gemalto-and-G-D-launch-Trustonic-for-TEE)
  - GlobalPlatform conforming
  - Used by [**MediaTek**](https://www.trustonic.com/news/company/mediatek-licences-trustonic-trusted-execution-environment/) and Samsungs **Eyxnos**
  - MicroKernel design (as stated [here](https://www.trustonic.com/news/technology/what-is-a-trusted-execution-environment-tee/))
- Trusted Logic (Gemalto company) `Trusted Foundations`
  - Unsure if used on Android
- TEEGRIS
  - [Samsung](https://developer.samsung.com/teegris/overview.html) (again!)    
- TZOS
  - Includes TEEGRIS, Kinibi, QSEE    


### Built on top of x86

//TODO

### Standards

- [Global Platform](https://www.globalplatform.org/)
  - GlobalPlatform is a non-profit, member driven association which defines and develops specifications to facilitate the secure deployment and management of multiple applications on secure chip technology. 
  - [Members](https://www.globalplatform.org/membershipcurrentfull.asp)

### Trustlets

Trustlets and small applications which are intended to run in the TEE. The TrustZone Kernel  can vary depending on the SoC but the TrustLet code should remain pretty much the same. 

The blog posts listed below talks about reversing Trustlets. They seem to be signed but not encrypted. Not sure if any SoCs support Trustlet encryption: seems like it would be a good way to prevent reversing!

## Trusted UIs

- [GlobalPlatform | Trusted User Interface Made Simple](https://www.globalplatform.org/mediaguidetrustedui.asp)
- [The Benefits of Trusted User Interface](https://www.trustonic.com/news/blog/benefits-trusted-user-interface/)
  - > This year Trustonic’s Trusted User interface (TUI) technology was integrated in Samsung’s flagship devices: the Galaxy Note 4 and Galaxy S6 – https://www.trustonic.com/products-services/trustonic-for-samsung-knox.  With this technology Trustonic’s Trusted Execution Environment (TEE) can process user interactions (display and touch) independently from Android in a hardware isolated enclave, made possible by ARM’s TrustZone™. 

## Vulns

- [Extracting Qualcomm's KeyMaster Keys – Breaking Android Full Disk Encryption](https://news.ycombinator.com/item?id=12007923) _30/6/2016_
  - Shows how the KEK can be lifted from Android devices and claims not fixable i.e. a hardware issue. Talks about how this means FDE (full disk encryption) is not bound to the device hardware, which is bad as it could easily be brute-forced off-device. 
  - Likely to have implications to `KeyStore` use
- Vuln in any Trustlet can allow the TEE to be compromised
  - (Re:Trusty) Although the Trusty OS enables the development of new applications, doing so must be exercised with extreme care; each new application increases the area of the trusted computing base (TCB) of the system. Trusted applications can access device secrets and can perform computations or data transformations using them.
- [INSPECTING DATA FROM THE SAFETY OF YOUR TRUSTED EXECUTION ENVIRONMENT](https://www.blackhat.com/docs/ldn-15/materials/london-15-Williams-Inspecting-Data-From-The-Safety-Of-Your-Trusted-Execution-Environment.pdf)
- [Trust Dies in Darkness: Shedding Light on Samsung’s TrustZone Keymaster Design](https://www.usenix.org/system/files/sec22fall_shakevsky.pdf)
  
## Common Usage Scenarios

- Verifying kernel integrity (TIMA)
- Using the Hardware Credential Storage (used by "keystore", "dm-verity")
- Secure Element Emulation for Mobile Payments
- Implementing and managing Secure Boot
- DRM (e.g. PlayReady)
- Accessing platform hardware features (e.g. hardware entropy)

## Glossary 

- `Secure World` - code that runs in a "Trusted" secure enviroment. In contrast to `Normal World`.
- `SMC` - Secure Monitor Calls
  - Bridge between Normal and Secure World.
- `SCM` - Secure Channel Manager
  - Qualcomm Linux Kernal driver that interacts with QSEE via SMC

## Deveopment 

- [Open-TEE - An Open Virtual Trusted Execution
Environment](https://arxiv.org/pdf/1506.07367.pdf)

## Links

- Bits Please Series
  - [1: Getting arbitrary code execution in TrustZone's kernel from any context](http://bits-please.blogspot.co.uk/2015/03/getting-arbitrary-code-execution-in.html)
    - Has intro to TrustZone
  - [2: Exploring Qualcomm's TrustZone implementation](http://bits-please.blogspot.co.uk/2015/08/exploring-qualcomms-trustzone.html)
    - Talks about the SMC driver opcode calls for `normal-world` to call into `secure-world`
  - [3: Exploring Qualcomm's Secure Execution Environment](http://bits-please.blogspot.co.uk/2016/04/exploring-qualcomms-secure-execution.html)
    - Talks about the Qualcomm's Secure Execution Environment (QSEE)
    - What trustlets are and how they are loaded
  - [4: QSEE privilege escalation vulnerability and exploit (CVE-2015-6639)](http://bits-please.blogspot.co.uk/2016/05/qsee-privilege-escalation-vulnerability.html)
    - Talks about the Linux kernel device, called "qseecom", which enables user-space processes to perform a wide range of TrustZone-related operations, such as loading trustlets into the secure environment and communicating with loaded trustlets.
    - Mainly focuses on the ability for exisiting system TrustLet vulns to expose the entire system memory to attack, even without a kernel vuln (focuses on Widevine DRM)
  - [Extracting Qualcomm's KeyMaster Keys - Breaking Android Full Disk Encryption](http://bits-please.blogspot.co.uk/2016/06/extracting-qualcomms-keymaster-keys.html)
    - Talks about how the FDE key is a software key KDFed from a SHK (hardware key) and can therefore be lifted by an OEM signed Trustlet, or a vuln in an existing Trustlet
- [Reflections on Trusting TrustZone](https://www.blackhat.com/docs/us-14/materials/us-14-Rosenberg-Reflections-on-Trusting-TrustZone.pdf)
- [Trusted Execution Environments (and Android)](https://usmile.at/sites/default/files/androidsecuritysymposium/presentations2015/Ekberg_AndroidAndTrustedExecutionEnvironments.pdf)
- [A software level analysis of TrustZone OS and Trustlets in Samsung Galaxy Phone](https://sensepost.com/blog/2013/a-software-level-analysis-of-trustzone-os-and-trustlets-in-samsung-galaxy-phone/)
  - Talks about MobiCore, how to deploy Trustlets, and how to blackbox assess them
- [ARM: Trusted Zone on Android\*](https://www.slideshare.net/lekaha/arm-trusted-zone-on-android)  
- [ARM TrustZone's Secure/Normal world vs x86's Ring0/3 or OS's kernel/user mode?](https://community.arm.com/developer/ip-products/processors/trustzone-for-armv8-m/f/trustzone-armv8-m-forum/4536/arm-trustzone-s-secure-normal-world-vs-x86-s-ring0-3-or-os-s-kernel-user-mode)

## Vulns

- [“ATTACK TRUSTZONE WITH ROWHAMMER” PRESENTATION SLIDES](http://www.eshard.com/2017/04/20/download-attack-trustzone-with-rowhammer-presentation-slides/)
