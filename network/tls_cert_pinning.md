#TLS Cert Pinning

There are various approaches to cert pinning on android. What they all have in common is that they validate the cert chain in use by looking for compile time known certs. Below is a TLDR of a simple approach as spoken about in the below links.

- Depending on the approach these can be Root, Intermediary, or Leaf (or a combo of the above) depending on requirements. 
- Pin the SHA256 of Subject Public Key Info (SPKI)
- "...developers should not check pins against the list of certificates sent by the server. Instead, pins should be checked against the new, 'clean' chain that is created during SSL validation."

#N Additions

Can pin via xml as shown [here](http://developer.android.com/preview/features/security-config.html#CertificatePinning).

#Links

- [Paypal Engineering - Key Pinning in Mobile Applications](https://www.paypal-engineering.com/2015/10/14/key-pinning-in-mobile-applications/)
- [Testing for CVE-2016-2402 and similar pinning issues](https://koz.io/pinning-cve-2016-2402/)
- [rfc7469 `Public Key Pinning Extension for HTTP`](https://tools.ietf.org/html/rfc7469) (HPKP)
- [rfc5280 Internet X.509 Public Key Infrastructure Certificate and Certificate Revocation List (CRL) Profile](https://tools.ietf.org/html/rfc5280)
