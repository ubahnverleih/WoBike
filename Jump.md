# JUMP (USA)

[JUMP](http://jumpbikes.com/) operates electric dockless bikeshares in Washington, DC & San
Francisco. They operate open data APIs at https://dc.jumpmobility.com/opendata
and https://sf.jumpbikes.com/opendata respectively.

Also see:

* https://github.com/Leschonander/Jump-Bike-D.C-Python-API-Wrapper
* https://github.com/salanza/node-jump



# How to get the Information
Other than for example Lime, Jump uses a series of reverse-engineering mitigations like certificate pinning.
Therefore, I write this little guide to make the process easier in the future.

## Requirements
* Android Device. The easiest would be to use an old device with Android 6 installed, since Man-in-the-Middle attacks are easier on older Android versions. Newer versions also work, but you will find how for yourself as I used a Nexus 5 with Android 6.0.1. Your device **must not be rooted**, as Jump (a.k.a. Uber) have a root detection. Android emulator also does not seem to work for some reason. You also have to [enable USB debugging on your device](https://upaae.com/how-to-enable-usb-debugging-in-android-6-0-1-marshmallow/).
* A recent version of [Apktool](https://ibotpeaches.github.io/Apktool/), which is used to decompile and repackage the app.
* [Split APK Installer (SAI)](https://github.com/Aefyr/SAI) on your device, which is used to a) download and b) reinstall the app from an to your device.
* [Android SDK Platform Tools](https://developer.android.com/studio/command-line/adb) or at least `adb` and `zipalign`.
* [Burp Suite Community Edition](https://portswigger.net/burp) for the actual MitM attack. You can use other tools, but again, as I used burp, the guide also relies on burp.
* A [Google Cloud Platform](https://console.cloud.google.com) API key, registered for Android Map SDK.

## Downloading the App
First, open the Play Store on your device, search for Jump and download the app.
After the download, do not open the app, but start SAI and export the Jump app.
On your computer, run `adb pull <PATH_TO_EXPORTED_JUMP_APP>`.
This will download the APK from the device to your computer.
As the Jump app is an app bundle, the file is a APKS rather than APK.
The next step is to unpack the APKS simply by running `unzip <PATH_TO.APKS>`.

## Preparation
### Burp Suite Proxy Preparation
First, we have to setup Burp Suite's proxy.
Therefore, start Burp and change to the Proxy tab.
First, turn off the HTTP intercept as we do not want to alter the traffic, but only capture it.
Second, change to the Options tab under Proxy.
Here, highlight the localhost proxy, click on Edit.
Change the port to something more generic like `9999`, and chose your computers IP address in the Spcific Address drop down.
Confirm with OK.
Cool, Burp is now waiting for your traffic.

### Burp Suite Proxy on Android
Ofcourse we need to tell Android to reroute all traffic to the proxy.
To do so, open the Settings on Android, go to Wi-Fi and long-press on the Wi-Fi network you are connected to.
A dialog will pop up.
Chose Edit Network.
Scroll down and you will see the proxy settings (which are set to none).
Change this to manual.
Now you can add you computers IP and the proxy's port to the respective field.
Apply these changes.
Now, all HTTP traffic is proxy'd through Burp, but HTTPS not yet.

To enable HTTPS, open a browser on the Android device and go to `http://burp`.
In the top right corner, you can download Burp's certificate.
Download it and rename it to `cert.cer`.
Now, go to Security in the Android Settings.
Here, you will find a option to install certificates from storage.
Open this option and select the downloaded `cert.cer` (not `cert.der`!).
Simply follow the instructions (you can choose what ever name you want).
To confirm that everything works well, open a browser on Android and visit any HTTPS page.
You should see all traffic in the HTTP history tab under the Proxy tab.

### Getting the Burp Certificate Fingerprint
When we bypass the certificate pinning, we have to replace Uber's certificate fingerprint with our own.
To do so, go to the Options tab under Proxy in Burp and export the CA certificate in DER format.
You have to specifiy an absolute path, otherwise the certificate won't be saved.
The next step is to convert the DEM certifcate to the PEM format.
This is done with the OpenSSL CLI, which should be available on all Linux and macOS systems anyways.

```
openssl x509 -inform der -in <PATH_TO_CERTIFICATE> -out <PATH_TO_NEW_CERTIFICATE>
```

After that, we want to gather the fingerprint.
This requires some more steps.
First, we want to get the public part from the certificate and extract the public RSA key.
This will be hashed and then base64 encoded.
Thanks to Unix' Pipes, this can be done in one command:

```
openssl x509 -in <PATH_TO_PEM_CERTIFICATE> -pubkey -noout | openssl rsa -pubin -outform der | openssl dgst -sha256 -binary | openssl enc -base64
```

There you have your base64 encoded certificate fingerprint.

### APK signing key
Android requires APKs to be signed.
Luckily, we can do this without any hasle ourself.
Java and Android ship everythin required to do this.
Let's create a signing key:

```
keytool -genkey -v -keystore <PATH_TO_KEYSTORE>.keystore -alias <KEYNAME> -keyalg RSA -keysize 2048 -validity 10000
```

Please remember the `KEYNAME`, as we need it later on.
You can chose what ever you want during the creation.
During the last question you have to type `yes`, otherwise, the process will start again.

This key will be used later to sign the APKs.

## Decompiling
Now it's time to decompile the app.
This requires multiple steps.
First, decompile all APK files that are not called `base.apk` using `apktool`:

```
apktool d -r <SPLIT_CONFIG_APK>
```

Run this command on all `split_config*` APK files.
The `-r` option hinders `apktool` to decompile resources, which would cause errors during repackaging.

Now comes the tricky part.
We have to decompile the `base.apk` twice
First, we have to path the `AndroidManifest.xml`.
Since the `-r` option also hinders `apktool` to decompile the manifest, we have to do this in a separate step.
Let's get started.
Decompile the `base.apk` including all resource files and rename the resulting folder into something meaningful:

```
apktool d base.apk -o base.complete/
```

Now, decompile the `base.apk`, but leave the resource files intact:

```
apktool d -r base.apk
```

## Patching
Now comes the actual patching.
What we basically want to do is to bypass Uber's certificate pinning.
Unfortunately, the Google Maps underlay stops to work since we inevitably alter the apps signature.
Therefore, we also have to add our own Google Maps API key to the app.

### Google Maps API key
This is what we do first.
Open the `AndroidManifest.xml` in `base.complete/` folder and search for the `com.google.android.geo.API_KEY` string.
Replace the `@string` value with your Google Maps API key.
Additionally, in one of the permissions in the manifest, there is a wrong space in the value string, which has to be removed (somewhere around line 5).

Now comes a tricky part.
The easy way would be to move the `AndroidManifest.xml` from `base.complete/` to `base/`.
But as we decompiled the `base/` folder with the `-r` flag, `apktool` will not compile the `AndroidManifest.xml` to the binary XML format required.
Therefore, we first recompile `base.complete/`, decompile it again but leave the manifest untouched this time, which then can be used to replace the original file.
Unfortunately, this is not that straight forward as it should be.
First, try to recompile the `base.complete/` folder with

```
apktool b base.complete/ -o base.tmp.apk
```

This will yield a number of errors denoting that multiple definitions of the same value exist in some resource files.
Unfortunately, you have to fix these errors manually.
Open the file with the error and remove duplicate entries, but leave at least one entry intact.
Now, retry the compilation.
You should now have a `base.tmp.apk` file.
As we need the compiled `AndroidManifest.xml`, we have to decompile the `base.tmp.apk` again, but leave the manifest untouched:

```
apktool d -r base.tmp.apk -o base.tmp/
```

Now copy the `AndroidManifest.xml` from `base.tmp/` to `base/`.

### Certificate Pinning
To bypass the certificate pinning, we need to find the fingerprint of Uber's certificates and replace them with our own.
`grep` for the string `sha256/` in all `.smali` files.
There should be one file with two base64 encoded SHA256 fingerprints.
Replace both occurences with the base64 encoded SHA256 fingerprint of your certificate.
Now, some lines below, you will see `*.uber.com`, which indicates, that the certificate should be valid for all Uber subdomains.
Simply replace this with `*`, which will tell the certificate pinning implementation to accept all domains.
Finally, replace the date string with the `valid until` field of your certificate.

Thats it for the patching part.
Let' recompile everything and install the app.

## Build and Deploy
### Build
To recompile the APKs, we just use `apktool` again:

```
apktool b -f base/ -o base.unaligned.apk
apktool b -f split_config<1>/ -o split_config<1>.unaligned.apk
apktool b -f split_config<2>/ -o split_config<2>.unaligned.apk
...
```

Note, that you have to use the `-f` flag, because `apktool` would ignore our patched `AndroidManifest.xml`.
As you may notice, the APKs have this `unaligned` in their names.
This will be used later, as we have to align the files.
But first, let's sign the APKs.

This is obviously done with the signing key and `jarsigner` from the [preparations](## Preparation) part:

```
jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore <PATH_TO_THE_KEYSTORE> -storepass <KEY_PASSWORD> <PATH_TO_UNALIGNED_APK> <NAME_OF_SIGNING_KEY>
```

Repeat this process for all the compiled APKs.

The final step before deployment is now to align the APKs with `zipalign`:

```
zipalign -v 4 <PATH_TO_UNALIGNED_APK> <OUTPUT_PATH_FOR_ALIGNED_APK>
```

Again, repeat this process for all unaligned and signed APKs.

### Deploy
Now, use `adb` and push all aligned and signed APKs to your device:

```
adb push <PATH_TO_BASE_APK> <PATH_TO_SPLIT_1_APK> <PATH_TO_SPLIT_2_APK> ... /sdcard
```

Now, open the SAI app on your device, go to settings and enable APK signing (the installed app bundle has to signed again, apparently).
Then, go back to the apps home screen and tap on install APKs.
Go the the location, where you pushed the APKs to, and select **all at once** and then tap install.
This will take a while.
After some minutes, you will be asked, if you want to install Jump.
Again, after some minutes, the installation should finish without any further notice.

## Final thoughts
Congrats.
You successfully bypassed Uber's anti-sniffing technique.
Unfortunately, the Jump app will only work when the Burp Proxy is enabled, as it now strictly requires Burp's certificate.
Now you can go to Burp and in the Proxy tab, you should see all traffic, including Uber's and Jump's traffic.

Unfortunately, this is a rather tedious task.
If someone finds an easier or faster way to achieve this, I would be very happy.
