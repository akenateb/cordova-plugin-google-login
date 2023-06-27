# cordova11-plugin-google-login

Cordova>=11 plugin modified for integrating Google Signin in Android (iOS app not TESTED yet) based on [russmedia-digital/cordova-plugin-google-signin](https://github.com/russmedia-digital/cordova-plugin-google-signin/tree/main). This plugin have been fully tested only on Android at this moment.

# Installation

## _First, we have to use Google API_
1.	Go to https://console.cloud.google.com/.

2.	Navigate to 'APIs & Services' > 'Credentials' > 'Create Credentials'.

3.	Select 'OAuth client ID'.

- App Type: Web Application
- Name: Choose any desired name

4.	Create the credentials.
•	Take note of the following information (copy and save it):
•	CLIENT_ID: 11378992162-mvimjrf60d3m6jr1h5v5658fge08n3d9.apps.googleusercontent.com

5.	Create the REVERSED_CLIENT_ID by reversing the order:
•	REVERSED_CLIENT_ID: com.googleusercontent.apps.11378992162-mvimjrf60d3m6jr1h5v5658fge08n3d9

Keep these credentials for later use.
## _Next, for Firebase:_
1.	Go to https://firebase.google.com/.

2.	Access the Firebase console at https://console.firebase.google.com/u/0/.

3.	Add a new project.
•	Name: Enter the desired project name.
•	Enable/Disable Google Analytics (optional)

4.	Create the project.

5.	To add Android to your project (iOS not tested yet):
•	Android project name: com.company.appname (You can find this data "applicationId" in 'output-metada.json'.)
•	Name for Firebase console (optional): Use the app name, for example.
•	SHA-1: Add your debug SHA-1 here. The production key will be added later.
•	Developer KEY: Open one terminal move to your project root folder and run: 

```sh
keytool -list -v -alias androiddebugkey -keystore "C:\Users\youruser\.android\debug.keystore" -storepass android -keypass android
```
•	Copy the SHA-1 line, it should look something like this: 

>E7:B3:5D:19:2F:30:DD:63:71:0C:92:OA:95:8E:93:D7:1C:BF:21:D5

If you do not have it:
Open terminal and run:

```sh
keytool -genkey -v -keystore debug.keystore -alias aliasappdebugkey -keyalg RSA -keysize 2048 -validity 10000
```

•	Production KEY: Open one terminal move to your project root folder and run: 

```sh
keytool -genkey -v -keystore filestoringkey.jks -keyalg RSA -keysize 2048 -validity 10000 -alias appname -storepass yourpassword
```


•	If you want to view your created key, Open one terminal move to your project root folder and run: 

```sh
keytool -list -v -keystore filestoringkey.jks -alias microGPT -storepass yourpassword
```

6.	Download the google-service.json file to (project)\platform\android\app.

7.	Adding the SDK:
•	Open (project)\platform\android\build.gradle.
•	Inside the 'buildscript' block, add the following line: classpath "com.google.gms:google-services:4.3.10". You should see something like this:
>buildscript {
    apply from: 'CordovaLib/cordova.gradle'
    apply from: 'repositories.gradle'
    repositories repos ///repos has google() and mavenCentral() once you have installed the plugin later
    dependencies {
        classpath "com.android.tools.build:gradle:${cordovaConfig.AGP_VERSION}";
        classpath "com.google.gms:google-services:4.3.10"; ////THIS IS NEW LINE
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:${cordovaConfig.KOTLIN_VERSION}"
    }


•	Open (project)\platform\android\app\build.gradle.
•	Apply the Google services plugin by adding this line: apply plugin: 'com.google.gms.google-services', You should see something like this:

>Line 20/21:
>apply plugin: 'com.android.application'
>apply plugin: 'com.google.gms.google-services'

•	In the dependencies section, add the following line: implementation platform('com.google.firebase:firebase-bom:32.1.1'). You should see something like this:

>dependencies {
    implementation fileTree(dir: 'libs', include: '*.jar')
    implementation "androidx.appcompat:appcompat:${cordovaConfig.ANDROIDX_APP_COMPAT_VERSION}"
    implementation "androidx.core:core-splashscreen:${cordovaConfig.ANDROIDX_CORE_SPLASHSCREEN_VERSION}"
    implementation platform('com.google.firebase:firebase-bom:32.1.1') ///THIS IS NEW LINE
    ...

8.	Once you have added SDK Click Next and Finish.

9.	On your project dashboard, click on 'Authentication' and then click on 'Start' or go directly to the 'Sign-in method' tab.

10.	Select 'Google' as the sign-in method under 'Additional providers'.

11.	Enable and select your project and your email, then save the changes.
That's all for Firebase.
## _Finally, install the plugin in your Cordova Project_
> make sure you have GIT installed on your system before proceeding



Open one terminal move to your project root folder and run:

```sh
cordova plugin add https://github.com/russmedia-digital/cordova-plugin-google-signin --save --variable REVERSED_CLIENT_ID=myreversedclientid --variable CLIENT_ID=yourclientid
```

Finally. Run on terminal:

```sh
cordova prepare
cordova run android
```

> It has been tested using terminal directly not a device emulator.

# Usage
```sh
//Get user Information
cordova.plugins.GoogleSignInPlugin.getUser(
 function (user) {
  console.log(user);
  },
  function (error) {
  console.error(error);
  }
);

cordova.plugins.GoogleSignInPlugin.signIn(
  function (authData) {
    // {
    //   "status": "success",
    //   "message": {
    //     "id": "",
    //     "display_name": "",
    //     "email": "",
    //     "photo_url": "",
    //     "id_token": ""
    //   }
    // }
    console.log(authData);
  },
  function (error) {
    console.error(error);
  }
);

cordova.plugins.GoogleSignInPlugin.isSignedIn(
  function (success) {
    console.log(success);
  },
  function (error) {
    console.error(error);
  }
);

cordova.plugins.GoogleSignInPlugin.signOut(
  function (success) {
    console.log(success);
  },
  function (error) {
    console.error(error);
  }
);

// Android only
cordova.plugins.GoogleSignInPlugin.oneTapLogin(
  function (success) {
    console.log(success);
  },
  function (error) {
    console.error(error);
  }
);
```
