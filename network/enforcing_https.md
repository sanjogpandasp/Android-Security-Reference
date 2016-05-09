
#Enforcing Https 

From [Protecting against unintentional regressions to cleartext traffic in your Android apps](https://security.googleblog.com/2016/04/protecting-against-unintentional.html)

- `android:usesCleartextTraffic=”false”`
- `StrictMode.VmPolicy.Builder.detectCleartextNetwork()` in development
