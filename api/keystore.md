#KeyStore

> KeyStore is responsible for maintaining cryptographic keys and their owners.
 
and 
 
> Use the Android Keystore provider to let an individual app store its own credentials that only the app itself can access. This provides a way for apps to manage credentials that are usable only by itself while providing the same security benefits that the KeyChain API provides for system-wide credentials. This method requires no user interaction to select the credentials.

##Related API

- [`KeyStore`](http://developer.android.com/reference/java/security/KeyStore.html)
  - KeyStore is responsible for maintaining cryptographic keys and their owners.
- Used with   
  - [`KeyPair`](http://developer.android.com/reference/java/security/KeyPair.html)
    - KeyPair is a container for a public key and a private key. 
  - [`KeyPairGenerator`](http://developer.android.com/reference/java/security/KeyPairGenerator.html)
    - KeyPairGenerator is an engine class which is capable of generating a private key and its related public key utilizing the algorithm it was initialized with. 
    - `KeyPairGenerator.getInstance(<cipherName>, "AndroidKeyStore");` for the key to be generated in the system keystore.
  - [`KeyPairGeneratorSpec`](http://developer.android.com/reference/android/security/KeyPairGeneratorSpec.html)
    - This provides the required parameters needed for initializing the KeyPairGenerator that works with Android KeyStore 
    - **This class was deprecated in API level 23. Use `KeyGenParameterSpec` instead.**
  - [`KeyGenerator`](https://developer.android.com/reference/javax/crypto/KeyGenerator.html)
    - This class provides the public API for generating symmetric cryptographic keys.
  - [`KeyGenParameterSpec`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
    - AlgorithmParameterSpec for initializing a KeyPairGenerator or a KeyGenerator of the Android Keystore system. 
    - **Since M-6-23**
    - More control over what the key can be used for and when
    - Authentiaction required support (lock screen | finger per use)
  - [`Cipher`](http://developer.android.com/reference/javax/crypto/Cipher.html#init(int, java.security.Key))
    - `Cipher.init(...)` takes a [`Key`](http://developer.android.com/reference/java/security/Key.html), which if is the output of `KeyPair.getPrivate()`, where `KeyPair` is obtained from `KeyStore.getEntry(...)`, then allows `Cipher` operations to be performed with a key that lives in the hardware/software keystore (win!). 
      - This is true as long as the `Cipher` config in question is supported by the hardware (if present). 
      - This list is found on the [Android Keystore System](http://developer.android.com/training/articles/keystore.html) page

##Version changes

- **D-1.6-4**
  - Keystore implemented as a native keystore daemon that used a local socket as its IPC interface
- **J-4.3-18**
  - [Credential storage enhancements in Android 4.3](http://nelenkov.blogspot.co.uk/2013/08/credential-storage-enhancements-android-43.html)
  - can store public / private keys via RSA `Cipher` support
  - `Signiture` support for RSA varients
  - can have multiple device users
    - before 4.3 only PRIMARY user could use
  - CANT store AES keys
    - but could encypt the AES key with created public / private key and in keystore
    - can store directly via hidden apis - see link above
    - Can with [`Vault`](https://android.googlesource.com/platform/development/+/master/samples/Vault/src/com/example/android/vault/SecretKeyWrapper.java) wrapper class
      - check out [g+ post](https://plus.google.com/+JeffSharkey/posts/9BmGb3xbPcA)
  - [Android 4.3 release notes](http://developer.android.com/about/versions/android-4.3.html#Security)
  - deamon retired and replaced with binder interface
- **K-4.4-19**
  - Support for elliptic curve
  - `Signiture` support for ECDSA varients
- **M-6-23**
  - First time for AES `Cipher` support (See ['Supported Algorithms'](http://developer.android.com/training/articles/keystore.html#SupportedAlgorithms)) 
  - `KeyGenerator` support for AES & HMAC
  - `KeyFactory` & `KeyPairGenerator` support for EC & RSA
  - `Mac` Hmac support
  - `SecretKeyFactory` support for AES & Hmac 
  - `Signature` increased support 
  - "Keys which do not require encryption at rest will no longer be deleted when secure lock screen is disabled or reset (for example, by the user or a Device Administrator). Keys which require encryption at rest will be deleted during these events.
  - [`KeyProtection`](http://developer.android.com/reference/android/security/keystore/KeyProtection.html)
    - Specification of how a key or key pair is secured when imported into the Android Keystore system.
  - `KeyStore` keys can require fingerprint auth before use 
    - "User authentication authorizes a specific cryptographic operation associated with one key. In this mode, each operation involving such a key must be individually authorized by the user". See `api/finger` for more.
- **N-7?-24?**
  - Key Attestion
    - Can prove to 3rd partys that a hardware keystore exists with certain keys by signing a representation using a factory supplied key 

##Authenticating Key Use

- Used in 3 User related modes
	- Not encrypted (default)
	  - If using the `KeyStore` without any additional settings just unlocking the device should unlock the `KeyStore` 
	- [`.setEncryptionRequired()`](http://developer.android.com/reference/android/security/KeyPairGeneratorSpec.Builder.html#setEncryptionRequired())  
	  - Requires a lock screen is set 
	- `KeyGenParameterSpec.Builder.setUserAuthenticationRequired(true)` 
	   - User has to autheticate each use of the key (see below for more)	   
	   - Optional `.setUserAuthenticationValidityDurationSeconds(AUTHENTICATION_DURATION_SECONDS)`
	     - Previous key validations are valid for this amount of time 
- OS access auth
  - Key access is tied to the apps UID
  - If rooted any user/app can in theory assume any UID
  - Access marshalled by keystore deamon on older apis and binder server on new ones 
- Crypto uses auth
  - authorized key algorithm, operations or purposes (encrypt, decrypt, sign, verify), padding schemes, block modes, digests with which the key can be used;   
 
###More on `.setUserAuthenticationRequired(true)` 

If a key has been created with `.setUserAuthenticationRequired(true)` then the user has to either authenticate via 

- [`KeyGuardManager.createConfirmDeviceCredentialIntent(...)`](https://developer.android.com/reference/android/app/KeyguardManager.html#createConfirmDeviceCredentialIntent(java.lang.CharSequence, java.lang.CharSequence)) to promt the user to enter pin, pattern or password. 
  - [ConfirmCredential](http://developer.android.com/samples/ConfirmCredential/index.html)
    - Uses `.setUserAuthenticationValidityDurationSeconds(AUTHENTICATION_DURATION_SECONDS)`  
    - This sample demonstrates how you can use device credentials (PIN, Pattern, Password) in your app to authenticate the user before they are trying to complete some actions
    - Will try to use key as part of crypto op after auth
- Fingerprint
  - Check [fingerprint.md](fingerprint.md) for more. Essentially needs to perform `KeyStore` op in fingerprint api auth callback.

##Losing Keys and how to handle

See [Android Security: The Forgetful Keystore](http://doridori.github.io/android-security-the-forgetful-keystore/) for losing Keys due to lock screen changes.

You can also lose Keys that require fingerprint auth when a new finger is enrolled. See [fingerprint.md](/api/fingerprint.md)

##Locking of Keystore

I have not encountered this myself but there are anecdontal reports of keystores becoming locked even when `setEncryptionRequired` == false. See [this SO post](http://stackoverflow.com/a/25790891/236743).

##Hardware vs Software

Some devices have hardware backed keystore storage, which was introduced in 4.3 but not manditory (Nexus devices since Nexus 4 have had this afaik). If a [CTS](http://static.googleusercontent.com/media/source.android.com/en//compatibility/android-cdd.pdf) compatable fingerprint functionality is available then

> MUST have a hardware-backed keystore implementation, and perform the fingerprint matching in a Trusted Execution Environment (TEE) or on a chip with a secure channel to the TEE.

As of M-6-23 hardware backed keystore is not a requirement, but this may soon change:

> Note that while the above TEE-related requirements are stated as STRONGLY RECOMMENDED, the
Compatibility Definition for the next API version is planned to changed these to REQIUIRED. If a
device implementation is already launched on an earlier Android version and has not implemented a
trusted operating system on the secure hardware, such a device might not be able to meet the
requirements through a system software update and thus is STRONGLY RECOMMENDED to
implement a TEE.

_Taken from [6.0 CTS](http://static.googleusercontent.com/media/source.android.com/en//compatibility/android-cdd.pdf) doc section 9.11. Keys and Credentials_

The presence of hardware backed key storage can be checked via the [`KeyChain.isBoundKeyAlgorithm`](http://developer.android.com/reference/android/security/KeyChain.html#isBoundKeyAlgorithm(java.lang.String)) method and for M-6-23+ [`KeyInfo.isInsideSecureHardware ()`](http://developer.android.com/reference/android/security/keystore/KeyInfo.html#isInsideSecureHardware()). See below

> Android also now supports hardware-backed storage for your KeyChain credentials, providing more security by making the keys unavailable for extraction. That is, once keys are in a hardware-backed key store (Secure Element, TPM, or TrustZone), they can be used for cryptographic operations but the private key material cannot be exported. Even the OS kernel cannot access this key material. While not all Android-powered devices support storage on hardware, you can check at runtime if hardware-backed storage is available by calling KeyChain.IsBoundKeyAlgorithm().

This means on a hardware backed device even if rooted the private keys cannot be extracted! They can still be used locally however (by an attacker).

> Key material may be bound to the secure hardware (e.g., Trusted Execution Environment (TEE), Secure Element (SE)) of the Android device. When this feature is enabled for a key, its key material is never exposed outside of secure hardware. If the Android OS is compromised or an attacker can read the device's internal storage, the attacker may be able to use any app's Android Keystore keys on the Android device, but not extract them from the device. This feature is enabled only if the device's secure hardware supports the particular combination of key algorithm, block modes, padding schemes, and digests with which the key is authorized to be used. To check whether the feature is enabled for a key, obtain a KeyInfo for the key and inspect the return value of KeyInfo.isInsideSecurityHardware().

and 

> If the Android OS is compromised or an attacker can read the device's internal storage, the attacker may be able to use any app's Android Keystore keys on the Android device, but not extract them from the device

The software blob can however be read on rooted devices. See [nelenkov/keystore-decryptor](https://github.com/nelenkov/keystore-decryptor).

##CAs

CAs are also stored in the `KeyStore`. When added a custom CA device should prompt user to enter device credentials.


##Links

- [`KeyStore`](http://developer.android.com/reference/java/security/KeyStore.html) class
- [developer.android.com] [Android Keystore System](http://developer.android.com/training/articles/keystore.html)
  - Shows list of Ciphers (& more) supported by the `KeyStore` 
- [Nikolay Elenkov] [Keystore redesign in Android M](https://nelenkov.blogspot.co.uk/2015/06/keystore-redesign-in-android-m.html)
- Bugs / Vulns
  - [Android keystore key leakage between security domains](http://jbp.io/2014/04/07/android-keystore-leak/)
    - Use `setEncryptionRequired()` to mitigate 


