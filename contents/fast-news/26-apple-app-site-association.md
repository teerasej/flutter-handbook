
# สร้างไฟล์ apple-app-site-association

สำหรับ iOS การทำงานของ Deep links จะเป็นรูปแบบของ Apple ที่เรียกว่า Universal Link จะเป็นการที่เราต้องมีการยืนยันความเป็นเจ้าของ domain name ที่จะผนวกเข้ากับตัวแอพพลิเคชั่น 

ซึ่งเราต้องอัพโหลดไฟล์ชื่อ **apple-app-site-association** ไปไว้ใน root directory ของ hosting ที่ผูกกับ Domain ดังกล่าว

โดยไฟล์ต้องสามารถเข้าถึงได้ตาม Pattern ต่อไปนี้ 

```
<domain name>/.well-known/apple-app-site-association
```

> ⚠️ ไม่ต้องมีนามสกุลไฟล์ .json

## A. ตัวอย่างไฟล์ apple-app-site-association

```json
{
    "applinks": {
        "apps": [],
        "details": [
            {
                "appID": "th.in.nextflow.fastnewsapp",
                "paths": [ "*" ]
            }
        ]
    }
}
```

2 จุดสำคัญ ที่ต้องนำมาใส่ไว้ในไฟล์นี้ คือ 

1. **package_name** สามารถเอามาได้จากไฟล์ android/app/build.gradle ในโปรเจค Flutter

```gradle
<!-- android/app/build.gradle -->
android {

    ....

    defaultConfig {
       
        applicationId "th.in.nextflow.fastnewsapp"
        ...
    }
```

2. **sha256_cert_fingerprints** ซึ่งสามารถใช้ตัวที่ลิ้งค์กับ App Bundle release ใน [Google Play Console](https://play.google.com/console/) ได้

