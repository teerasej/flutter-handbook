

# การตั้งค่า Permission สำหรับระบบ Android

## 1. minimum SDK version

เปิดไฟล์ `android/app/build.gradle` และตรวจสอบค่า `minSdkVersion` ให้เป็น 21 หรือสูงกว่า

```gradle
android {
  defaultConfig {
    ...
    minSdkVersion 24
  }
}
```

## 2. (optional) การเพิ่ม permission สำหรับกล้อง

เปิดไฟล์ `android/app/src/main/AndroidManifest.xml` และเพิ่ม permission สำหรับกล้องดังนี้

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    // Add the permission here:
    <uses-permission android:name="android.permission.CAMERA" />
    <application>
    ...
```
