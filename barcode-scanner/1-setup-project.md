
# Setup 

## 1. ดาวน์โหลดและเตรียมโปรเจค

- สามารถดาวน์โหลด zip ไฟล์จาก[ลิ้งค์นี้](https://github.com/teerasej/nextflow_flutter_scanner/tree/starter)
- หรือใช้คำสั่ง clone โปรเจคด้วย git ด้านล่างก็ได้ 

```bash
git clone -b stater https://github.com/teerasej/nextflow_flutter_scanner
```

เปิดโปรเจคแล้วรันคำสั่ง `flutter package get` ให้เรียบร้อย

> ถ้าเปิดบน Visual Studio Code ก็อย่าลืมสั่ง get package ให้เรียบร้อย


## 2. รายชื่อ Package

ลองตรวจดูไฟล์ pubspec.yaml จะเห็นรายชื่อ package สำหรับโปรเจคนี้

```yaml
dependencies:
  ...
  flutter_barcode_scanner: ^1.0.2
```

- **flutter_barcode_scanner** สำหรับเรียกใช้ตัวแสกน และอ่านค่ากลับมาใช้งาน