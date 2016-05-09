#Overview to AOSP api components

Below there is info on the `KeyStore`

> KeyStore is responsible for maintaining cryptographic keys and their owners.

and the `KeyChain`

> Use the KeyChain API when you want system-wide credentials. When an app requests the use of any credential through the KeyChain API, users get to choose, through a system-provided UI, which of the installed credentials an app can access. This allows several apps to use the same set of credentials with user consent.

#KeyStore

version changes
	1.6+
		Keystore implemented as a native keystore daemon that used a local socket as its IPC interface
	4.3
		Credential storage enhancements in Android 4.3
			example of how to create local key pair
		can store public / private keys
		can have multiple device users
			before 4.3 only PRIMARY user could use
		CANT store AES keys
			but could encypt the AES key with created public / private key and in keystore
			can store directly via hidden apis - see link above
			Can with VAULT wrapper class
				check out g+ post
		android release notes
		deamon retired and replaced with binder interface
	4.4
		support for elliptic curve

#KeyChain

- "...application access to the system key store and allows users to grant application access to the credentials stored there."
- 4.0+ api for cred storage. 4.3 hardware support
