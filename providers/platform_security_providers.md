#Platform Security Providers

_The below could be completly wrong, please correct if any mistakes noticed_

Security functionality on Android is provided by various open src libs which vary depending on the OS version.

##OpenSSL

- OpenSSL version used - look in AOSP openSSL version file for each platform tag 4.3 used 1.0.1e
- OpenSSL introduced in 4.2?

##AndroidOpenSSL

- AndroidOpenSSL highest priority since 4.4
- AndroidOpenSSL was introduced in 4
- AndroidOpenSSL covers most of BouncyCastles functionality in 4.4
- AndroidOpenSSL since 4.4 has been pulled out of core and can be used as a standalone lib (conscrypt)
 
##Java JCE Providers

###BouncyCastle

- Android used an old version of BC ("[crippled]")
- Was the default security Java Security API provider
- Package was renamed at 3.0. Before this issues if including a newer version of BC (hence [SC](https://rtyley.github.io/spongycastle/))

###SpongyCastle

- Many people import this so can use latest BC. 
- If going to use best not to make default but still set staically so can use with direct sepcifier throughout app

###None

- N has removed all JCE security providers! See ...

##Harmony
		
- Old and being phased out
	
##BoringSSL
		
- Google patched fork of openSSL
- Transition starting in 6.0



