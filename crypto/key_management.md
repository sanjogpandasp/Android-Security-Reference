#Overview to AOSP api components

- `KeyStore`
  - "KeyStore is responsible for maintaining cryptographic keys and their owners."
- `KeyChain`
  - "...application access to the system key store and allows users to grant application access to the credentials stored there."
  - 4.0+ api for cred storage. 4.3 hardware support
  - "Use the KeyChain API when you want system-wide credentials. When an app requests the use of any credential through the KeyChain API, users get to choose, through a system-provided UI, which of the installed credentials an app can access. This allows several apps to use the same set of credentials with user consent."
