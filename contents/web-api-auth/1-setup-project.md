
# Setup 

## 1. ดาวน์โหลดและเตรียมโปรเจค

- สามารถดาวน์โหลด zip ไฟล์จาก[ลิ้งค์นี้](https://github.com/teerasej/nextflow_note_client_with_authen/tree/stater-focus-request-authen)
- หรือใช้คำสั่ง clone โปรเจคด้วย git ด้านล่างก็ได้ 

```bash
git clone -b stater-focus-request-authen https://github.com/teerasej/nextflow_note_client_with_authen
```

เปิดโปรเจคแล้วรันคำสั่ง `flutter package get` ให้เรียบร้อย

> ถ้าเปิดบน Visual Studio Code ก็อย่าลืมสั่ง get package ให้เรียบร้อย


## 2. รายชื่อ Package

ลองตรวจดูไฟล์ pubspec.yaml จะเห็นรายชื่อ package สำหรับโปรเจคนี้

```yaml
dependencies:
  ...
  flutter_secure_storage: ^3.3.5
  dio: ^3.0.10
```

- **[flutter_secure_storage](https://pub.dev/packages/flutter_secure_storage)** สำหรับเก็บ token ที่ได้จากการล๊อคอิน เพื่อใช้ในการติดต่อกับ Web API
- **[dio](https://pub.dev/packages/dio)** ตัวช่วยในการส่ง Request และรับ Response 