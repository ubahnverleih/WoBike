# JUMP (USA) (defunct)

[JUMP](http://jumpbikes.com/) operates electric dockless bikeshares and scooter is various cities around the globe.
To get bike and scooter data, you need to authenticate yourself.
Unfortunately, this is a) rather tedious that requires 5 (yes, five) steps and b) you have to register a phone number using the Jump app.
I assume, that you already have an account, as I don't know how to do this programmatically.

## UPDATE

**JUMP Scooters are now found under the [Lime](Lime.md) API as `"brand": "jump"`**

## Getting an API token
This task is quite heavy and requires 5 steps and parsing HTML and JavaScript source code.

### 0: Helper
During the process, Uber sends some HTML and JavaScript files, which you need to parse.
I wrote a little Python helper to do this.
Give it the `index.html` file as a parameter, it will store all tokens required from the file in `tokens.txt`.
Basically, this is a [CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery)-[token](https://en.wikipedia.org/wiki/Cross-site_request_forgery#Cookie-to-header_token) and a session ID.
Both change after each step, therefore, you have to run the Python script after every step (as stated later).
For simplicity, save the Python helper in a file called `parse.py`.

```Python
#! /usr/bin/env python3

import sys
from html.parser import HTMLParser

token_path = "tokens.txt"

class MyHTMLParser(HTMLParser):
    def handle_starttag(self, tag, attrs):
        if tag != 'input':
            return

        if len(attrs) != 3:
            return

        if attrs[1][1] == 'sess':
            with open(token_path, 'a') as token_file:
                token_file.write(f"sess={attrs[2][1]}\n")

        if attrs[1][1] == 'x-csrf-token':
            with open(token_path, 'a') as token_file:
                token_file.write(f"csrf={attrs[2][1]}\n")

        if attrs[1][1] == 'inAuthSessionID':
            with open(token_path, 'a') as token_file:
                token_file.write(f"auth={attrs[2][1]}\n")


tmp = open(token_path, 'w')
tmp.close()

parser = MyHTMLParser()
with open(sys.argv[1], 'r') as html_file:
    parser.feed(html_file.read())

```

### 1: Request Authentication
The first request will ask the Uber backend for authentication and start the process.
This will also create a cookie jar file `cookie.jar`, which is required during the next steps.

```sh
curl -X GET \
    -H "Accept-Encoding: gzip, deflate" \
    --cookie-jar "cookie.jar" \
    --url "https://auth.uber.com/login/?uber_client_name=jump" \
    | gzip -dc \
    | tee index.html \
    && python3 parse.py index.html
```

Again, you should have three files now available, `cookie.jar` with the cookie, `tokens.txt` with a CSRF token and a session ID and `index.html`, where this information is from.

### 2: Submitting Phone Number
If you have a look in the `index.html` from the previous step, you will see, that Uber is requesting a phone number.
This is what we are gonna do now.

```sh
CSRF=$(grep csrf tokens.txt | cut -d '=' -f2 | sed 's/ *$//')
SESS=$(grep sess tokens.txt | cut -d '=' -f2 | sed 's/ *$//')

COUNTRY_CODE="49"   # two digit ISO country code without + or 0
PHONE="1234567"     # Phone number without leading 0 or country code.

curl -X POST \
    -H "Accept-Encoding: gzip, deflate" \
    --data "countryCode=$COUNTRY_CODE" \
    --data "phoneNumber=$PHONE" \
    --data "autoSMSVerificationSupported=false" \
    --data "uberClientName=jump" \
    --data "type=INPUT_MOBILE" \
    --data "x-csrf-token=$CSRF" \
    --data "sess=$SESS" \
    -b "cookie.jar" \
    --cookie-jar "cookie.jar" \
    --url "https://auth.uber.com/login/session" \
    | gzip -dc \
    | tee index.html \
    && python3 parse.py index.html
```

This will a) update the three files (`index.html`, `tokens.txt`, `cookie.jar`) and b) request a 4-digit 2FA code that is sent to your phone number and that you will need in the next step.

### 3: Confirm 2FA Code
Now we want to sent the 2FA code to uber.

```sh
CSRF=$(grep csrf tokens.txt | cut -d '=' -f2 | sed 's/ *$//')
SESS=$(grep sess tokens.txt | cut -d '=' -f2 | sed 's/ *$//')
AUTH=$(grep auth tokens.txt | cut -d '=' -f2 | sed 's/ *$//')

CODE="1234" # 4-digit 2FA code

curl -X POST \
    -H "Accept-Encoding: gzip, deflate" \
    --data "type=SMS_OTP" \
    --data "autoSMSVerificationSupported=false" \
    --data "uberClientName=jump" \
    --data "smsOTP=$CODE" \
    --data "x-csrf-token=$CSRF" \
    --data "sess=$SESS" \
    --data "inAuthSessionID=$AUTH" \
    -b "cookie.jar" \
    --cookie-jar "cookie.jar" \
    --url "https://auth.uber.com/login/session" \
    | gzip -dc \
    | tee index.html \
    && python3 parse.py index.html
```

Alright, almost done.
Again, the three files are updated again.

### 4: Password
For this step, you have to provide your password.

```sh
CSRF=$(grep csrf tokens.txt | cut -d '=' -f2 | sed 's/ *$//')
SESS=$(grep sess tokens.txt | cut -d '=' -f2 | sed 's/ *$//')
AUTH=$(grep auth tokens.txt | cut -d '=' -f2 | sed 's/ *$//')

PW="YOUR_PW_HERE"

curl -X POST \
    -H "Accept-Encoding: gzip, deflate" \
    --data "type=VERIFY_PASSWORD" \
    --data "autoSMSVerificationSupported=false" \
    --data "uberClientName=jump" \
    --data "password=$PW" \
    --data "x-csrf-token=$CSRF" \
    --data "sess=$SESS" \
    --data "inAuthSessionID=$AUTH" \
    -b "cookie.jar" \
    --url "https://auth.uber.com/login/session" \
    | sed -nE 's/.*#code=(.*)&in_auth_session_id=.*/\1/p'
```

We are getting close.
The last `sed` command will strip everything away except a UUID with the usual `123abcde-abcd-01234-abcd-123456789abc` format.
You will need this for the last step.

### 5: Confirmation
Basically, you will need the UUID from the previous step to confirm you login:

```sh
UUID="123abcde-abcd-01234-abcd-123456789abc"

curl -X POST \
    -H "Accept-Encoding: gzip, deflate" \
    -H "Connection: close" \
    -H "Content-Type: application/json; charset=UTF-8" \
    --data "{\"formContainerAnswer\":{\"inAuthSessionID\":\"$UUID\",\"formAnswer\":{\"flowType\":\"SIGN_IN\",\"screenAnswers\":[{\"screenType\":\"SESSION_VERIFICATION\",\"fieldAnswers\":[{\"fieldType\":\"SESSION_VERIFICATION_CODE\",\"sessionVerificationCode\":\"$UUID\"}]}]}}}" \
    --url "https://cn-geo1.uber.com/rt/silk-screen/submit-form" \
    | gzip -dc
```

Now, you will get a JSON response including various IDs and tokens.
What you need is the `apiToken`, again, looking a regular UUID `abcde123-abcd-01234-abcd-123456789abc`.
Congratulations.
You did it.
This `apiToken` is what you need to get scooters and bikes.

## Requesting vehicles
To request vehicles, there is one known endpoint so far.

```sh
API_TOKEN="abcde123-abcd-01234-abcd-123456789abc"   # API Token from authentication

curl -X POST \
    -H "x-uber-token: $API_TOKEN" \
    -H "Content-Type: application/json; charset=UTF-8" \
    -H "Accept-Encoding: gzip, deflate" \
    --data '{"latitude":52.528038680440716,"longitude":13.401972334831953,"radius":1000000}' \
    --url "https://cn-geo1.uber.com/rt/emobility/search-assets" | gzip -dc
```

There are only three parameters: `latitude`, `longitude` of a center point and `radius` around this point.
You can set the radius as you wish, but there seems to be a maximum at about 500 meters, if I see correctly.
You will get a JSON response including all information about vehicles.


# How to get the Information (a.k.a. Reverse Engineering an Android App)
Other than for example Lime, Jump uses a series of reverse-engineering mitigations like certificate pinning.
Therefore, I write this little guide to make the process easier in the future.

## Requirements
* Android Device. The easiest would be to use an old device with Android 6 installed, since Man-in-the-Middle attacks are easier on older Android versions. Newer versions also work, but you will find how for yourself as I used a Nexus 5 with Android 6.0.1. Your device **must not be rooted**, as Jump (a.k.a. Uber) have a root detection. Android emulator also does not seem to work for some reason. You also have to [enable USB debugging on your device](https://upaae.com/how-to-enable-usb-debugging-in-android-6-0-1-marshmallow/).
* A recent version of [Apktool](https://ibotpeaches.github.io/Apktool/), which is used to decompile and repackage the app.
* [Split APK Installer (SAI)](https://github.com/Aefyr/SAI) on your device, which is used to a) download and b) reinstall the app from an to your device.
* [Android SDK Platform Tools](https://developer.android.com/studio/command-line/adb) or at least `adb` and `zipalign`.
* [Burp Suite Community Edition](https://portswigger.net/burp) for the actual MitM attack. You can use other tools, but again, as I used burp, the guide also relies on burp.
* A [Google Cloud Platform](https://console.cloud.google.com) API key, registered for Android Map SDK.
* I used version `2.39.10000` for this. Chances are, that things change over time. I will try to keep this guide up-to-date, but you know. Upkeep is always hard.

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
Of course we need to tell Android to reroute all traffic to the proxy.
To do so, open the Settings on Android, go to Wi-Fi and long-press on the Wi-Fi network you are connected to.
A dialog will pop up.
Chose Edit Network.
Scroll down and you will see the proxy settings (which are set to none).
Change this to manual.
Now you can add your computers IP and the proxy's port to the respective field.
Apply these changes.
Now, all HTTP traffic is proxy'd through Burp, but HTTPS not yet.

To enable HTTPS, open a browser on the Android device and go to `http://burp`.
In the top right corner, you can download Burp's certificate.
Download it and rename it to `cert.cer`.
Now, go to Security in the Android Settings.
Here, you will find an option to install certificates from storage.
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

```sh
openssl x509 -inform der -in <PATH_TO_CERTIFICATE> -out <PATH_TO_NEW_CERTIFICATE>
```

After that, we want to gather the fingerprint.
This requires some more steps.
First, we want to get the public part from the certificate and extract the public RSA key.
This will be hashed and then base64 encoded.
Thanks to Unix' Pipes, this can be done in one command:

```sh
openssl x509 -in <PATH_TO_PEM_CERTIFICATE> -pubkey -noout \
    | openssl rsa -pubin -outform der \
    | openssl dgst -sha256 -binary \
    | openssl enc -base64
```

There you have your base64 encoded certificate fingerprint.

### APK signing key
Android requires APKs to be signed.
Luckily, we can do this without any hasle ourself.
Java and Android ship everythin required to do this.
Let's create a signing key:

```sh
keytool -genkey \
    -v -keystore <PATH_TO_KEYSTORE>.keystore \
    -alias <KEYNAME> \
    -keyalg RSA \
    -keysize 2048 \
    -validity 10000
```

Please remember the `KEYNAME`, as we need it later on.
You can chose what ever you want during the creation.
During the last question you have to type `yes`, otherwise, the process will start again.

This key will be used later to sign the APKs.

## Downloading the App
First, open the Play Store on your device, search for Jump and download the app.
After the download, do not open the app, but start SAI and export the Jump app.
On your computer, run `adb pull <PATH_TO_EXPORTED_JUMP_APP>`.
This will download the APK from the device to your computer.
As the Jump app is an app bundle, the file is a APKS rather than APK.
The next step is to unpack the APKS simply by running `unzip <PATH_TO.APKS>`.

## Decompiling
Now it's time to decompile the app.
This requires multiple steps.
First, decompile all APK files that are not called `base.apk` using `apktool`:

```sh
apktool d -r <SPLIT_CONFIG_APK>
```

Run this command on all `split_config*` APK files.
The `-r` option hinders `apktool` to decompile resources, which would cause errors during repackaging.

Now comes the tricky part.
We have to decompile the `base.apk` twice.
First, we have to patch the `AndroidManifest.xml`.
Since the `-r` option also hinders `apktool` to decompile the manifest, we have to do this in a separate step.
Let's get started.
Decompile the `base.apk` including all resource files and rename the resulting folder into something meaningful:

```sh
apktool d base.apk -o base.complete/
```

Now, decompile the `base.apk`, but leave the resource files intact:

```sh
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

```sh
apktool b base.complete/ -o base.tmp.apk
```

This will yield a number of errors denoting that multiple definitions of the same value exist in some resource files.
Unfortunately, you have to fix these errors manually.
Open the file with the error and remove duplicate entries, but leave at least one entry intact.
Now, retry the compilation.
You should now have a `base.tmp.apk` file.
As we need the compiled `AndroidManifest.xml`, we have to decompile the `base.tmp.apk` again, but leave the manifest untouched:

```sh
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

```sh
apktool b -f base/ -o base.unaligned.apk
apktool b -f split_config<1>/ -o split_config<1>.unaligned.apk
apktool b -f split_config<2>/ -o split_config<2>.unaligned.apk
...
```

Note, that you have to use the `-f` flag, because `apktool` would ignore our patched `AndroidManifest.xml`.
As you may notice, the APKs have this `unaligned` in their names.
This will be used later, as we have to align the files.
But first, let's sign the APKs.

This is obviously done with the signing key and `jarsigner`:

```sh
jarsigner -verbose \
    -sigalg MD5withRSA \
    -digestalg SHA1 \
    -keystore <PATH_TO_THE_KEYSTORE> \
    -storepass <KEY_PASSWORD> \
    <PATH_TO_UNALIGNED_APK> \
    <NAME_OF_SIGNING_KEY>
```

Repeat this process for all the compiled APKs.

The final step before deployment is now to align the APKs with `zipalign`:

```sh
zipalign -v 4 <PATH_TO_UNALIGNED_APK> <OUTPUT_PATH_FOR_ALIGNED_APK>
```

Again, repeat this process for all unaligned but signed APKs.

### Deploy
Now, use `adb` and push all aligned and signed APKs to your device:

```sh
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
