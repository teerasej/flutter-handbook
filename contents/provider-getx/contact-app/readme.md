
# Contact App with GetX

## Starter project บน Github

- [Start](https://github.com/teerasej/nextflow_flutter_3_contact_app/tree/start)
- [Finish](https://github.com/teerasej/nextflow_flutter_3_contact_app/tree/finish)

หรือใช้คำสั่งด้านล่างในการ clone มา

```bash
git clone -b start ttps://github.com/teerasej/nextflow_flutter_3_contact_app
```
หรือเลือกสร้างโปรเจคใหม่ โดยใช้ชื่อว่า `contact_app`

## Step by Step

1. [ติดตั้ง GetX และ setup](1-setup-getx.md)
2. [สร้างหน้าเพิ่ม contact ใหม่](2-new-contact-page.md)
3. [สร้าง controller และใช้ในการเก็บข้อมูลจากแบบฟอร์ม](3-controller-form.md)
4. [แสดงข้อความเตือนเมื่อไม่ได้กรอกข้อมูลในแบบฟอร์ม](4-validation.md)
5. [แสดงรายชื่อ contact ผู้ติดต่อในหน้า home](5-contact-controller.md)

## [SQFLite](https://pub.dev/packages/sqflite) 

### Stater project

- ทำการ download ไฟล์ zip จาก [Start project](https://github.com/teerasej/nextflow_flutter_3_contact_app/tree/finish) มาเป็นโปรเจคตั้งต้น
- หรือใช้คำสั่งด้านล่างในการ clone มา

```bash
git clone -b finish https://github.com/teerasej/nextflow_flutter_3_contact_app
```

หากพบปัญหาในการรันแอพบน Android ด้วย error `'ThemeData' is from 'package:flutter/src/material/theme_data.dart' ` ให้[ลองแก้ไขปัญหาด้วยวิธีนี้](https://github.com/teerasej/nextflow_flutter_3_contact_app/issues/2#issuecomment-2143506201) 

1. [เพิ่ม package ที่จำเป็นในโปรเจค](6-sqlite.md)
2. [สร้างกลไก สร้าง database file และ schema ตอน controller เริ่มทำงาน](7-sqlite-controller.md)
3. [เพิ่มข้อมูลลงใน SQLite database](8-insert-sqlite.md)
4. [แสดงข้อมูลจาก SQLite database ในหน้า home](9-show-sqlite.md)

- [Finish project](https://github.com/teerasej/nextflow_flutter_3_contact_app/tree/add-sqlite-functionality)

## Trouble shooting

หากมีปัญหาเรื่อง Java Version, Gradle, ACP ตอนรันโปรเจคให้ลองใช้ hotfix [ตรงนี้ดูครับ](https://github.com/flutter/flutter/issues/156304#issuecomment-2397707812)

```
pluginManagement {
    def flutterSdkPath = {
        def properties = new Properties()
        file("local.properties").withInputStream { properties.load(it) }
        def flutterSdkPath = properties.getProperty("flutter.sdk")
        assert flutterSdkPath != null, "flutter.sdk not set in local.properties"
        return flutterSdkPath
    }()

    includeBuild("$flutterSdkPath/packages/flutter_tools/gradle")

    repositories {
        google()
        mavenCentral()
        gradlePluginPortal()
    }
}

plugins {
    id "dev.flutter.flutter-plugin-loader" version "1.0.0"
    id "com.android.application" version "8.3.2" apply false
    id "org.jetbrains.kotlin.android" version "2.0.20" apply false
}

include ":app"
```
