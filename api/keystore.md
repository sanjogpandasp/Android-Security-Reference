Below there is info on the [`KeyStore`](http://developer.android.com/reference/java/security/KeyStore.html)

> KeyStore is responsible for maintaining cryptographic keys and their owners.

#[KeyStore](http://developer.android.com/reference/java/security/KeyStore.html)

##Version changes

- 1.6
  - Keystore implemented as a native keystore daemon that used a local socket as its IPC interface
- 4.3
  - [Credential storage enhancements in Android 4.3](http://nelenkov.blogspot.co.uk/2013/08/credential-storage-enhancements-android-43.html)
  - can store public / private keys
  - can have multiple device users
    - before 4.3 only PRIMARY user could use
  - CANT store AES keys
    - but could encypt the AES key with created public / private key and in keystore
    - can store directly via hidden apis - see link above
    - Can with [`Vault`](https://android.googlesource.com/platform/development/+/master/samples/Vault/src/com/example/android/vault/SecretKeyWrapper.java) wrapper class
      - check out [g+ post](https://plus.google.com/+JeffSharkey/posts/9BmGb3xbPcA)
  - [Android 4.3 release notes](http://developer.android.com/about/versions/android-4.3.html#Security)
  - deamon retired and replaced with binder interface
- 4.4
  - support for elliptic curve
- 6.0
  - "Keys which do not require encryption at rest will no longer be deleted when secure lock screen is disabled or reset (for example, by the user or a Device Administrator). Keys which require encryption at rest will be deleted during these events.
  - [`KeyProtection`](http://developer.android.com/reference/android/security/keystore/KeyProtection.html)
    - Specification of how a key or key pair is secured when imported into the Android Keystore system.
  - `KeyStore` keys can require fingerprint auth before use 
    - "User authentication authorizes a specific cryptographic operation associated with one key. In this mode, each operation involving such a key must be individually authorized by the user". See `api/finger` for more.
- N (7?)
  - Key Attestion
    - Can prove to 3rd partys that a hardware keystore exists with certain keys by signing a representation using a factory supplied key 

##Authenticating Key Use

- [ConfirmCredential](http://developer.android.com/samples/ConfirmCredential/index.html)
    - This sample demonstrates how you can use device credentials (PIN, Pattern, Password) in your app to authenticate the user before they are trying to complete some actions
    - Will try to use key as part of crypto op after auth
- Auth Via Fingerprint
  - check [fingerprint.md](fingerprint.md) for more 

##Losing Keys

See [Android Security: The Forgetful Keystore](http://doridori.github.io/android-security-the-forgetful-keystore/)

##Links

- - [Nikolay Elenkov] [Keystore redesign in Android M](https://nelenkov.blogspot.co.uk/2015/06/keystore-redesign-in-android-m.html)


