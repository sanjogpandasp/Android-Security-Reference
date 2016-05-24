#Https

##Enforcing Https 

From [Protecting against unintentional regressions to cleartext traffic in your Android apps](https://security.googleblog.com/2016/04/protecting-against-unintentional.html)

- `android:usesCleartextTraffic=”false”` in manifest for M+ (6.0+). [Docs](https://developer.android.com/guide/topics/manifest/application-element.html#usesCleartextTraffic)
- `StrictMode.VmPolicy.Builder.detectCleartextNetwork()` in development

##Debugging

Starting in N can add custom trust anchors via xml which are used only for debug builds. Removes the vuln of accidentially leaving debug code in production. [IO link](https://youtu.be/XZzLjllizYs?t=1405)
