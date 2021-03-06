#Platform Security Providers

Security functionality on Android is provided by various open src libs which vary depending on the OS version. There are native libs and java libs with JCE interfaces which coexist. 

Guide to terms like JCE & JSSE can be found [here](http://www.oracle.com/technetwork/java/javase/tech/index-jsp-136007.html).

##Native Implementations

####OpenSSL

- Used since the beginning?
- OpenSSL version used - look in AOSP openSSL version file for each platform tag i.e. 4.3 used 1.0.1e
	
####BoringSSL

- [Source link](https://boringssl.googlesource.com/boringssl/)		
- "BoringSSL is a fork of OpenSSL that is designed to meet Google’s needs."
- Transition starting in 6.0

##Java JCE Providers

####Apache Harmonys Crypto Provider
		
- Old and being phased out
- Limited JCE functionality

####AndroidOpenSSL

- Introduced in 4 (SSL function only)
- Highest priority since 4.4
- Covers most of the same function as BC since 4.4
- Since 4.4 has been pulled out of core and can be used as a standalone lib (conscrypt)
- Backed by OpenSSL ( BoringSSL >= 6 ) natively
- Contained in the **Conscrypt** namespace (`com.android.org.conscrypt` on Android)
	- AndroidOpenSSL's [`Provider` def](https://android.googlesource.com/platform/external/conscrypt/+/android-n-preview-2/src/main/java/org/conscrypt/OpenSSLProvider.java#34) @ N preview v2
	- AndroidOpenSSL's [SPI interface definition](https://android.googlesource.com/platform/external/conscrypt/+/android-n-preview-2/src/main/java/org/conscrypt)  @ N preview v2
- Was part of `libcore` until 4.4 

####BouncyCastle

- Android used an old version of BC ("[crippled]")
- Was the default security Java Security API provider
- Package was renamed at 3.0. Before this issues if including a newer version of BC (hence [SC](https://rtyley.github.io/spongycastle/))
- Offered full JCE functionality

####SpongyCastle

- Many people import this so can use latest BC. 
- If going to use best not to make default but still set staically so can use with direct sepcifier throughout app

####None?

- ~~N has removed all JCE security providers! See the [N changes doc](https://github.com/doridori/Android-Security-Reference/blob/master/changes/N.md)~~ As mentioned in the link this information was removed after the annoucement!

##Updating platform Provider from your app

See (Updating Your Security Provider to Protect Against SSL Exploits)[http://developer.android.com/training/articles/security-gms-provider.html]. Not clear if updates `BoringSSL` or just the Java `AndroidOpenSSL` SPI (or both). 

