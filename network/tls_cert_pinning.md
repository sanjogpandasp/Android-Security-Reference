#TLS Cert Pinning

There are various approaches to cert pinning on android. What they all have in common is that they validate the cert chain in use by looking for compile time known certs. Below is a TLDR of a simple approach as spoken about in the below links.

- Depending on the approach these can be Root, Intermediary, or Leaf (or a combo of the above) depending on requirements. 
- Pin the SHA256 of Subject Public Key Info ([SPKI](https://tools.ietf.org/html/rfc5280#section-4.1.2.7))
- "...developers should not check pins against the list of certificates sent by the server. Instead, pins should be checked against the new, 'clean' chain that is created during SSL validation.". See Blog Posts below.

#N Additions

Can pin via xml as shown [here](http://developer.android.com/preview/features/security-config.html#CertificatePinning).

#Links

- Blog posts
  - [Paypal Engineering - Key Pinning in Mobile Applications](https://www.paypal-engineering.com/2015/10/14/key-pinning-in-mobile-applications/)
  - [Testing for CVE-2016-2402 and similar pinning issues](https://koz.io/pinning-cve-2016-2402/)
    - Talks about [An Examination of Ineffective Certificate Pinning Implementations](https://www.cigital.com/blog/ineffective-certificate-pinning-implementations/) 
  - [Okhttp pinning discussion](https://github.com/square/okhttp/issues/173) 
- RFCs
  - [rfc7469 `Public Key Pinning Extension for HTTP`](https://tools.ietf.org/html/rfc7469) (HPKP TOFU pin)
  - [rfc5280 Internet X.509 Public Key Infrastructure Certificate and Certificate Revocation List (CRL) Profile](https://tools.ietf.org/html/rfc5280)
- Guides  
  - [OWASP Public Key Pinning](https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning)
    - Has Android examples
- OpenSrc 
  - [moxie0/AndroidPinning](https://github.com/moxie0/AndroidPinning)
    - Has manual inclusion of system TrustManager 
  - [iSECPartners/android-ssl-bypass](https://github.com/iSECPartners/android-ssl-bypass)

#Random Notes

- When creating a `TrustManagerFactory` pass in null to get system `TrustManager`. Can pin while extending system TrustManager. 
