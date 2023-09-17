
# Setup Firebase ก่อน

1. [วิธีติดตั้ง Firebase ใช้งานในโปรเจค Google Flutter](../firebase-firestore/project-setup.md)
2. [การ setup firebase_auth](../firebase-firestore/firebase_auth.md)

## Android 

สำหรับ android อาจจะต้องมีการไปปรับ `android/build.gradle` ตามด้านล่าง 

```
buildscript {
    ext.kotlin_version = '1.7.10'
    repositories {
        google()
        mavenCentral()
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:7.3.0'
        // START: FlutterFire Configuration

        // ตรงนี้ถ้าเห็นเป็น 4.3.10 ให้เปลี่ยนเป็น 4.3.14 และบันทึกไฟล์
        classpath 'com.google.gms:google-services:4.3.14'

        // END: FlutterFire Configuration
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
}
```

เพราะมีเรื่องของ compatability ระหว่าง gradle กับ google-service ้อ้างอิงจาก https://stackoverflow.com/q/72224454 
