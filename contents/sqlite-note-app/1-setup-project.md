
# Setup 

## 1. ดาวน์โหลดและเตรียมโปรเจค

- สามารถดาวน์โหลด zip ไฟล์จาก[ลิ้งค์นี้](https://github.com/teerasej/nextflow_flutter_2_sqlite_note_app/tree/starter)
- หรือใช้คำสั่ง clone โปรเจคด้วย git ด้านล่างก็ได้ 

```bash
git clone -b starter https://github.com/teerasej/nextflow_flutter_2_sqlite_note_app
```

เปิดโปรเจคแล้วรันคำสั่ง `flutter package get` ให้เรียบร้อย

> ถ้าเปิดบน Visual Studio Code ก็อย่าลืมสั่ง get package ให้เรียบร้อย


## 2. รายชื่อ Package

ลองตรวจดูไฟล์ pubspec.yaml จะเห็นรายชื่อ package สำหรับโปรเจคนี้

```yaml
dependencies:
  ...
  sqflite: ^2.0.0+2
  path_provider: ^2.0.1
```

- **[sqflite](https://pub.dev/packages/sqflite)** สำหรับจัดการไฟล์ฐานข้อมูล รวมถึงการอ่าน และบันทึกข้อมูล
- **[path_provider](https://pub.dev/packages/path_provider)** สำหรับอ้างอิงตำแหน่งไฟล์ฐานข้อมูลในแอพ