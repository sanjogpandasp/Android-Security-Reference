#Pinning

There are various approaches to cert pinning on android. Below is a TLDR of a simple approach as spoken about in the below links.

-  Pin the SHA256 of Subject Public Key Info (SPKI)

#N Additions

Can pin via xml as shown [here](http://developer.android.com/preview/features/security-config.html#CertificatePinning).

#Links

- [Paypal Engineering - Key Pinning in Mobile Applications](https://www.paypal-engineering.com/2015/10/14/key-pinning-in-mobile-applications/)
- [rfc7469 `Public Key Pinning Extension for HTTP`](https://tools.ietf.org/html/rfc7469) (HPKP)
- [rfc5280 Internet X.509 Public Key Infrastructure Certificate and Certificate Revocation List (CRL) Profile](https://tools.ietf.org/html/rfc5280)
