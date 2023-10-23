# Framework Permissions

## References

- [cs.android.com] [Framework Permissions](https://cs.android.com/android/platform/superproject/main/+/main:frameworks/base/core/res/AndroidManifest.xml)
- [source.android.com] [Permissions Overview](https://source.android.com/docs/core/permissions)
- [android.googlesource.com] [Android permissions for system developers](https://android.googlesource.com/platform/frameworks/base/+/master/core/java/android/permission/Permissions.md)

### Permissions Groups

Permission grouping affect the way the permissions request is shown to the user. If a user has given permission for a permission in a group, other permissions in that group with be automatically granted to the requesting application.

See a list of permission groups here [Permission groups](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups)

## android:protectionLevels

See comprehensive official list [R.attr list](https://developer.android.com/reference/android/R.attr.html#protectionLevel). Notes below:

### Apk definable:

- `Normal`
  - The system automatically grants this type of permission to a requesting application at installation
- `Dangerous`
  - This type of permission introduces potential risk, the system may not automatically grant it to the requesting application. On M-6-23 (when the target is also 23+) this would trigger a dialog for the user to grant the perm 
- `Signature`
  - Meaning of this depends on where the permission is defined. 
    - If in the core OS [/system/framework/framework-res.apk/AndroidManifest.xml](https://github.com/android/platform_frameworks_base/blob/master/core/res/AndroidManifest.xml) then the permission holder would need to be signed by the same key (which is the _`platform`_ key used in the OS signing process)
    - If an Application defines this permission then the holder would need to be signed by the same key as the application
  - Permission will be auto granted if sig check passes 
  - [Changes were introduced in Android 12](https://developer.android.com/about/versions/12/features#safer-grant-signature-perms) to make granting signature permissions to other applications more flexible
    - _Starting in Android 12, the knownCerts attribute for signature-level permissions allows you to refer to the digests of known signing certificates at declaration time._ 
- `signatureOrSystem` 
  - See `signiture` and `system`
  
### Other:

- `System`
  - An app that resides in `system/app` (<4.4) or `system/priv-app/` (4.4+) [link](http://stackoverflow.com/a/20104400/236743) 
  - [Now deprecated](https://developer.android.com/reference/android/content/pm/PermissionInfo.html#PROTECTION_FLAG_SYSTEM) 
  - [Old synonym for "privileged".](https://developer.android.com/reference/android/R.attr.html#protectionLevel)
- `Privileged`
  - An app that resides in `system/priv-app/` (4.4+)  
  - Superceeds `system`
- `Preinstalled` 
  - An app that resides in `system/app` (4.4+)? [link](http://stackoverflow.com/questions/33481730/difference-between-preinstalled-and-privileged-protection-level)
- `appop`
  - Additional flag from base permission type: this permission is closely associated with an app op for controlling access.
  - e.g. [`SYSTEM_ALERT_WINDOW`](https://commonsware.com/blog/2017/05/11/system_alert_window-updates.html)
- [Various](https://developer.android.com/reference/android/R.attr.html#protectionLevel)

## Platform permission holder representation

When an app is installed the applications permissions are added to the below files. If you edit these on a rooted device the changes are ignored - and seemed to be wiped (or still ignored) on reboot (tested on 4.4.2 emulator). 

The `PackageManager` most likely has this info in mem and it may be verified on boot or have more verification checks down the protected call chains. Need to look into this more.

### `/data/system/packages.xml`

Entry looks like

```xml
<package name="com.example.android.apis" codePath="/data/app/ApiDemos.apk" nativeLibraryPath="/data/app-lib/ApiDemos" flags="4767300" ft="154bb1bf808" it="154bb1bf808" ut="154bb1bf808" version="19" userId="10050">
    <sigs count="1">
        <cert index="0" />
    </sigs>
    <perms>
        <item name="android.permission.READ_EXTERNAL_STORAGE" />
        ...
    </perms>
    <signing-keyset identifier="2" />
</package>
```

### `/data/system/packages.list`

Entry looks like

```
com.example.android.apis 10050 0 /data/data/com.example.android.apis default 3003,1028,1015
```

For info on the GIDs at the end look at [Kernel Permissions](kernel_perms.md)

# Permission Requesting

## 6 

[Overlays not allowed](https://i.stack.imgur.com/BEGzf.png)

## 7 

SYSTEM_ALERT_OVERLAY is auto removed when any permissions dialog is shown



