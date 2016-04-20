#Verified Boot

> Verified Boot, introduced in Android 4.4, provides a hardware-based root of
trust, and confirms the state of each stage of the boot process. During boot,
Android warns the user if the operating system has been modified from the
factory version, provides information about what the warning means, and
offers solutions to correct it. Depending on device implementation, Verified
Boot will either allow the boot to proceed, stop the device from booting so
the user can take action on the issue, or prevent the device from booting up
until the issue is resolved. Starting from Android 6.0, device implementations
with Advanced Encryption Standard (AES) crypto performance above 50MiB/
seconds support Verified Boot for device integrity.

> Details on Android Verified Boot implementation and features can be found
in the [Verified Boot](https://source.android.com/security/verifiedboot/index.html) section on source.android.com.

_From [Android Security 2015 Year in Review](http://static.googleusercontent.com/media/source.android.com/en//security/reports/Google_Android_Security_2015_Report_Final.pdf)_
