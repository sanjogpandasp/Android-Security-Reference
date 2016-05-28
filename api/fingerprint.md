#Fingerprint Support

Added in M-6-23, AOSP now officially supports fingerprint hardware with an API also available to applications.

##API

- Official Examples
  - [FingerPrintDialog](http://developer.android.com/samples/FingerprintDialog/src/com.example.android.fingerprintdialog/MainActivity.html) 
    - This sample demonstrates how you can use registered fingerprints to authenticate the user before proceeding some actions such as purchasing an item.
    - Will try to use key as part of crypto op after auth
    - Also lives in Githib as [googlesamples/android-FingerprintDialog](https://github.com/googlesamples/android-FingerprintDialog)
- [`FingerPrintManager`](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
  - A class that coordinates access to the fingerprint hardware. 
    - Check if supported
    - Check if fingers enrolled
    - Requesting Auth 
  - [`FingerPrintManager.authenticate`](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#authenticate(android.hardware.fingerprint.FingerprintManager.CryptoObject, android.os.CancellationSignal, int, android.hardware.fingerprint.FingerprintManager.AuthenticationCallback, android.os.Handler))
    - Warms up the sensor and will make callback once fingerprint made 
    - Can show own dialog. Recommended to use official finger icon
    - Crypto keys should be valid during lifecycle of succesful callback
- [`KeyGenParameterSpec.Builder.setUserAuthenticationRequired(boolean)`](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html#setUserAuthenticationRequired(boolean))
	- ...if the key requires that user authentication takes place for every use of the key (see [`setUserAuthenticationValidityDurationSeconds(int)`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html#setUserAuthenticationValidityDurationSeconds(int))), at least one fingerprint must be enrolled (see `hasEnrolledFingerprints()`)
- `USER_FINGERPRINT` permission needed
	- [`normal`](http://developer.android.com/reference/android/Manifest.permission.html#USE_FINGERPRINT) level permission, therefore no need to request at runtime

##`KeyStore` keys requiring auth

> User authentication authorizes a specific cryptographic operation associated with one key. In this mode, each 
operation involving such a key must be individually authorized by the user. Currently, the only means of such authorization 
is fingerprint authentication: FingerprintManager.authenticate. Such keys can only be generated or imported if at least one
fingerprint is enrolled (see FingerprintManager.hasEnrolledFingerprints). **These keys become permanently invalidated once a 
new fingerprint is enrolled or all fingerprints are unenrolled**.

Losing Keys after new finger enrollment

>The key will become irreversibly invalidated once the secure lock screen is disabled (reconfigured to None, Swipe or other mode which does not authenticate the user) or when the secure lock screen is forcibly reset (e.g., by a Device Administrator). Additionally, if the key requires that user authentication takes place for every use of the key, it is also irreversibly invalidated once a new fingerprint is enrolled or once\ no more fingerprints are enrolled. Attempts to initialize cryptographic operations using such keys will throw `KeyPermanentlyInvalidatedException`.

Taken from [`setUserAuthenticationRequired()`](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html#setUserAuthenticationRequired(boolean)). More info about Key creation can be found in [keystore.md](/api/keystore.md).


##AOSP CTS Requirements

To have google apps & services OEM devices must pass the CTS. 
[M-6-23 Compatability Doc](http://static.googleusercontent.com/media/source.android.com/en//compatibility/android-cdd.pdf)

> MUST have a hardware-backed keystore implementation, and perform the fingerprint matching in a Trusted Execution Environment (TEE) or on a chip with a secure channel to the TEE.

> MUST have a false acceptance rate not higher than 0.002%.

##3rd party OEM APIs

Some OEMs have introduced their own fingerprint API/hardware outside of the CTS running Android versions pre M-6-23. These include:

- Samsung ('[Pass](http://developer.samsung.com/release-note/view.do?v=R000000009)' API availble to devs)
- Oneplus (Properitary API not yet available as of Aug '15)
- HTC 
- ...

Its unknown if each OEM will maintain their own API as well as AOSPs when running M-6-23+. Samsung [now supports both](http://www.androidcentral.com/galaxy-s7-and-s7-edge-support-both-marshmallow-and-samsung-fingerprint-apis).

Some 3rd party (non-CTS?) implementations have been [shown to be vulnerable](http://www.engadget.com/2015/08/05/android-fingerprint-readers-may-be-easier-to-hack-than-touch-id/) however. 

##Risk of using Biometric?

There are a few articles saying using biometric is very risky as if you lose your fingerprint data you cant change your 
fingers. Using fingerprint on Android will never allow access to any print data directly and can only be directly be 
used for auth perposes local to the device. In terms of backing a crypto key, really all your doing is proving you
have the device in hand that you originally set up the key data on. Of course if you lose your device (or its severly pwned) 
_and_ someone clones your fingerprint then this security measure is bypassed. In this case you would want to revoke access to the service and just get another device.

Official link [Fingerprint security on Nexus devices](https://support.google.com/nexus/answer/6300638?hl=en-GB)

##Links

- [Official] [New in Android Samples: Authenticating to remote servers using the Fingerprint API](http://android-developers.blogspot.co.uk/2015/10/new-in-android-samples-authenticating.html?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed:+blogspot/hsDu+(Android+Developers+Blog))
  - Talks about `.setUserAuthenticationRequired(true)` 
- [Android: Fingerprint Authentication Thoughts](https://medium.com/@manuelvicnt/android-fingerprint-authentication-f8c7c76c50f8#.htn7xmypk)

##Libs 

- [square/Whorlwind](https://github.com/square/whorlwind)
  - A reactive wrapper around Android's fingerprint API that handles encrypting/decrypting sensitive data using a fingerprint.
