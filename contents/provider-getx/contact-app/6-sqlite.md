# เพิ่ม package ที่จำเป็นในโปรเจค

ในบทนี้เราจะเพิ่ม package ที่จำเป็นในการใช้งาน SQLite ในโปรเจค

เปิดไฟล์ `pubspec.yml` และเพิ่ม package `sqflite` และ `path_provider` ดังนี้:

> ให้แน่ใจว่าเวอร์ชั่นของ getx เป็นเวอร์ชั่นใหม่ หรือตามในภาพ

```yaml
dependencies:
  flutter:
    sdk: flutter

  cupertino_icons: ^1.0.2
  get: ^4.7.2
  
  # เพิ่ม package ที่จำเป็นในการใช้งาน SQLite
  sqflite: any
  path_provider: any
```

เสร็จแล้วบันทึกไฟล์
