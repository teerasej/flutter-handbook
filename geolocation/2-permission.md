
# กำหนด Permission

## Android 

**android/app/src/profile/AndroidManifest.xml**

```xml
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```


## iOS 

**ios/Runner/Info.plist**

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Needed to access location</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Needed to access location</string>
```