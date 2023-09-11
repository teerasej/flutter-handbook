
# ติดตั้ง package http สำหรับรับส่งข้อมูลกับ Web API 

1. เปิด Terminal ขึ้นมาใน Visual Studio Code 
2. รันคำสั่ง เพื่อดาวน์โหลด [http package](https://pub.dev/packages/http) มาจากอินเตอร์เน็ต 

```bash
flutter pub add http
```
3. ทดลองเปิดไฟล์ **pubspec.yaml** ที่อยู่ในโปรเจค สังเกตว่าจะมีชื่อ http อยู่ในส่วน dependencies 

```yaml
dependencies:
  flutter:
    sdk: flutter

  cupertino_icons: ^1.0.2
  http: ^0.13.3
```