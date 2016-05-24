#Sec Change Summary for N Preview

## Key Attestation

Can prove to 3rd partys that a hardware keystore exists with certain keys by signing a representation using a factory 
supplied key

> To ensure that the device is using a secure, official Android factory image, Key Attestation requires that the device bootloader provide the following information to the Trusted Execution Environment (TEE)...

[from](http://developer.android.com/preview/api-overview.html#key_attestation) 

##Network Security Config

> In Android N, apps can customize the behavior of their secure (HTTPS, TLS) connections safely, without any code modification, by using the declarative Network Security Config instead of using the conventional error-prone programmatic APIs (e.g. X509TrustManager)
  - Custom trust anchors.
  - Debug-only overrides. 
  - Cleartext traffic opt-out. 
  - Certificate pinning. 

[from](http://developer.android.com/preview/api-overview.html#network_security_config) 

- Custom trust achors 
  - can be specified for debug use only
  - trust root for domain(s) (so no need to purchase a cert)

## APK signature scheme v2

> The PackageManager class now supports verifying apps using the APK signature scheme v2. The APK signature scheme v2 is a whole-file signature scheme that significantly improves verification speed and strengthens integrity guarantees by detecting any unauthorized changes to APK files.

[from](http://developer.android.com/preview/api-overview.html#network_security_config)

## Direct Boot

> Android N runs in a secure, Direct Boot mode when the device has been powered on but the user has not unlocked the device.

Introduces

- _Credential encrypted storage_, which is the default storage location and only available after the user has unlocked the device.
- _Device encrypted storage_, which is a storage location available both during Direct Boot mode and after the user has unlocked the device.

[from](http://developer.android.com/preview/features/direct-boot.html)

## Crypto security provider has been removed.  

> You should only call to the Java Cryptography Extension (JCE) APIs with a provider listed if the provider is included in the code of the APK. Otherwise, your app needs to be able to handle the provider’s absence.

> The reason apps use this provider is to take advantage of its SecureRandom implementation. If your app was relying on setSeed() to derive keys from strings, you must either switch to using SecretKeySpec to load raw key bytes directly, or use a real key derivation function (KDF).

The above is not entirely correct as `Cipher` is backed by the sec provider, i.e. not only `SecureRandom`

This was originally copied from [the N release notes](http://developer.android.com/preview/behavior-changes.html#open-jdk) but has since been removed. It was found under the heading _Platform Migration toward OpenJDK 8_.
