
# Setup 

## 1. ดาวน์โหลดและเตรียมโปรเจค

- สามารถดาวน์โหลด zip ไฟล์จาก[ลิ้งค์นี้](https://github.com/teerasej/flutter_nextflow_mobile_scanner_2/tree/start)
- หรือใช้คำสั่ง clone โปรเจคด้วย git ด้านล่างก็ได้ 

```bash
git clone -b start https://github.com/teerasej/flutter_nextflow_mobile_scanner_2/
```

เปิดโปรเจคแล้วรันคำสั่ง `flutter package get` ให้เรียบร้อย

> ถ้าเปิดบน Visual Studio Code ก็อย่าลืมสั่ง get package ให้เรียบร้อย


## 2. รายชื่อ Package

ลองตรวจดูไฟล์ pubspec.yaml จะเห็นรายชื่อ package สำหรับโปรเจคนี้

```yaml
dependencies:
  ...
  mobile_scanner: 6.0.7
```
หรือรันคำสั่งด้านล่าง

```bash
flutter pub add mobile_scanner
```

- **[mobile_scanner](https://pub.dev/packages/mobile_scanner)** สำหรับเรียกใช้ตัวแสกน และอ่านค่ากลับมาใช้งาน

