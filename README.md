# Amplitude Air Native Extension

Adobe Air Native Extension for [Amplitude](https://amplitude.com/) analytics on the iOS and Android platforms.

## Version

This extension uses the Amplitude SDK version `5.1.0` for iOS and SDK version `2.25.1` for Android.

## Requirements

This extension requires iOS 7.0 or higher and Android 4.0 (API level 14) or higher.

When packaging your app for iOS, you need to use AIR 28+ or provide a path to iOS 11+ SDK (available in Xcode 9) using the `-platformsdk` option in `adt` or via corresponding UI of your IDE, for example:

```
-platformsdk /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS11.2.sdk
```

## Using the Extension

### Setup

Download the ANE from the [bin](bin/) directory or from the [releases](../../releases/) page and add it to your app's descriptor:

```xml
<extensions>
    <extensionID>com.digitalstrawberry.nativeExtensions.ANEAmplitude</extensionID>
</extensions>
```

On iOS, you may need to add the following ANE to be able to build your IPA:

* `com.distriqt.Core` (https://github.com/distriqt/ANE-Core)

If you are targeting Android, add the following dependency extensions:

* `androidx.core` (https://github.com/distriqt/ANE-AndroidSupport)
* `com.distriqt.playservices.AdsIdentifier` (https://github.com/distriqt/ANE-GooglePlayServices)
* `com.distriqt.playservices.Base` (https://github.com/distriqt/ANE-GooglePlayServices)
* `com.distriqt.square.okhttp3` (https://github.com/distriqt/ANE-SquareLibs)

> Credits to [Distriqt](https://github.com/distriqt) for providing these extensions.

You will also need to add the following permissions to the Android manifest:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
```

And add the following meta data to the `<application>` element inside manifest additions:

```xml
<meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
```

There is no additional code required for the iOS platform.

### Initialize

Start off by initializing the Amplitude extension with your API key:

```as3
Amplitude.initialize("YOUR_API_KEY");
```

### Set User Id (Optional)

You can set the client user id if you wish to identify specific users:

```as3
Amplitude.setUserId("USER_ID");
```

Alternatively you can also specify a user id when you initialize the extension:

```as3
Amplitude.initialize("YOUR_API_KEY", "USER_ID");
```

### Tracking Sessions

Session tracking is automatically handled for both iOS and Android. You do not need to call any methods for this to work correctly. To disable automatic tracking, pass in `false` as a third parameter to the `initialize` method:

```as3
Amplitude.initialize("YOUR_API_KEY", "USER_ID", false);
```

### Log Events

You can log a basic event with only a name:

```as3
Amplitude.logEvent("EVENT_NAME");
```

Or you can log complex events with multiple parameters:

```as3
Amplitude.logEvent("EVENT_NAME", {param1: "hello", param2: "world"});
```

### Set User Properties

You can set properties for the specific user:

```as3
Amplitude.setUserProperties({param1: "hello", param2, "world"});
```

### Set Group

You can assign user to a group. The second `groupName` parameter accepts either a string or an array of strings:

```as3
Amplitude.setGroup("organizationId", "15");

Amplitude.setGroup("sport", ["tennis", "baseball"]);
```

### Set Group Properties

Just like setting user properties, you can also set group properties:

```as3
Amplitude.setGroupProperties("company", "Amplitude", {members: 80});
```

### Log Revenue

Log a basic revenue event, with a quantity of one and a price of $1.99:

```as3
Amplitude.logRevenue("com.myproduct", 1, 1.99);
```

If you want to be able to use Amplitude's built-in revenue verification, you must extract the returned receipt data from your in app purchase extension that you are using. Use the following code for iOS:

```as3
Amplitude.logRevenue("com.myproduct", 1, 1.99, "INSERT_RECEIPT_DATA");
```

Android verification requires the IAP purchase data and data signature:

```as3
Amplitude.logRevenue("com.myproduct", 1, 1.99, "INSERT_PURCHASE_DATA", "INSERT_PURCHASE_SIGNATURE");
```

Note that the revenue must be converted to USD before it can be passed to Amplitude, as they do not currently offer currency conversion.


### Device Id

You can view the device indentifier Amplitude is using by calling the following:

```as3
var deviceId:String = Amplitude.getDeviceId();
```

By default Android uses a randomly generated UUID and iOS used the IDFV indentifier. If you would like Amplitude to use Google's Advertising ID on Android and Apple's IDFA on iOS, call the following method:

```as3
Amplitude.useAdvertisingIdForDeviceId();
```

## Further Documentation

You can learn more about the inner workings of the Amplitude system using the following resources:

* [Amplitude Documentation](https://amplitude.com/docs)
* [Amplitude iOS SDK](https://github.com/amplitude/Amplitude-iOS)
* [Amplitude Android SDK](https://github.com/amplitude/Amplitude-Android)

## Compiling from Source

You can compile the Air Native Extension from source by copying the `build/build.config.template` file to `build/build.config` updating it to match your development enviroment. Then run the `ant` command in the `build` folder to build the entire project.

## Special Thanks

Special thanks to the [StickSports](https://github.com/StickSports/) ANEs for the initial build script and testing code and the [ANEZipFile](https://github.com/xperiments/ANEZipFile) project for the FRETypeUtils code for iOS.