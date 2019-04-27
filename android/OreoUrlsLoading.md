# Android Oreo or later versions not loading URLs (cleartext traffic not permitted)

## Solution 1
Create a XML `res/xml/network_security_config.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <base-config cleartextTrafficPermitted="true" />
</network-security-config>
```
Reference this file in your tag `Application`, inside `AndroidManifest.xml`. Like:
`android:networkSecurityConfig="@xml/network_security_config"`

## Solution 2

Create file `res/xml/network_security_config.xml` -
```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <domain-config cleartextTrafficPermitted="true">
        <domain includeSubdomains="true">Your URL(ex: 127.0.0.1)</domain>
    </domain-config>
</network-security-config>
```

AndroidManifest.xml -
```<?xml version="1.0" encoding="utf-8"?>
<manifest ...>
    <uses-permission android:name="android.permission.INTERNET" />
    <application
        ...
        android:networkSecurityConfig="@xml/network_security_config"
        ...>
        ...
    </application>
</manifest>```

## Solution 3

AndroidManifest.xml -
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ...>
    <uses-permission android:name="android.permission.INTERNET" />
    <application
        ...
        android:usesCleartextTraffic="true"
        ...>
        ...
    </application>
</manifest>
```
The `android:targetSandboxVersion` can be a problem too -

According to [Manifest Docs of android:targetSandboxVersion](https://developer.android.com/guide/topics/manifest/manifest-element#targetSandboxVersion),

> The target sandbox for this app to use. The higher the sandbox version number, the higher the level of security. Its default value is 1; you can also set it to 2. Setting this attribute to 2 switches the app to a different SELinux sandbox. The following restrictions apply to a level 2 sandbox:
The default value of usesCleartextTraffic in the Network Security Config is false.
Uid sharing is not permitted.

## Solution 4

If you have `android:targetSandboxVersion` in `<manifest>` then reduce it to 1

AndroidManifest.xml -
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest android:targetSandboxVersion="1">
    <uses-permission android:name="android.permission.INTERNET" />
    ...
</manifest>
```
