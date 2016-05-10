#Fingerprint Support

Added in M-6-23, AOSP now officially supports fingerprint hardware with an API also available to applications.

##`KeyStore` keys requiring auth

> User authentication authorizes a specific cryptographic operation associated with one key. In this mode, each 
operation involving such a key must be individually authorized by the user. Currently, the only means of such authorization 
is fingerprint authentication: FingerprintManager.authenticate. Such keys can only be generated or imported if at least one
fingerprint is enrolled (see FingerprintManager.hasEnrolledFingerprints). These keys become permanently invalidated once a 
new fingerprint is enrolled or all fingerprints are unenrolled.

##Risk of using Biometric?

There are a few articles saying using biometric is very risky as if you lose your fingerprint data you cant change your 
fingers. Using fingerprint on Android will never allow access to any print data directly and can only be directly be 
used for auth perposes local to the device. In terms of backing a crypto key, really all your doing is proving you
have the device in hand that you originally set up the key data on. Of course if you lose your device (or its severly pwned) 
_and_ someone clones your fingerprint then this security measure is bypassed. In this case you would want to revoke access to the service and just get another device.

Official link [Fingerprint security on Nexus devices](https://support.google.com/nexus/answer/6300638?hl=en-GB)

##Links

- [Official] [New in Android Samples: Authenticating to remote servers using the Fingerprint API](http://android-developers.blogspot.co.uk/2015/10/new-in-android-samples-authenticating.html?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed:+blogspot/hsDu+(Android+Developers+Blog))
