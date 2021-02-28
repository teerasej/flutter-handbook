
# สร้าง Token Manager

token เหมือนบัตรผ่านที่ Web API จะให้เราหลังจาก login แล้ว ในที่นี้เราจะสร้าง Class ขึ้นมาเพื่อเรียกใช้ และเก็บ Token จากที่ต่างๆ ในแอพเราโดยเฉพาะ

## 1. สร้าง Class

```dart
// token_manager.dart
class TokenManager {
  
  // เก็บ token ไว้ใช้งานในส่วนต่างๆ ระหว่างที่แอพทำงานอยู่
  static String userToken;

  // เช็คว่ามี token เก็บอยู่ในเครื่องหรือไม่
  static Future<bool> isTokenExist() async {
    return false;
  }

  // อ่าน token ออกไปใช้งาน
  static Future<String> getToken() async {
    return '';
  }

  // บันทึก token 
  static Future<void> saveToken(String token) async {
    
  }
}
```
## 2. จัดการ token ด้วย flutter_secure_storage

ในที่นี้เราเลือก [Flutter secure storage](https://pub.dev/packages/flutter_secure_storage) เพื่อเก็บรักษาไว้ในเครื่อง เพราะมีความปลอดภัยสูงกว่าการเก็บข้อมูลทั่วไป

```dart
// token_manager.dart

import 'package:flutter_secure_storage/flutter_secure_storage.dart';

class TokenManager {
  static final storage = new FlutterSecureStorage();

  static String userToken;

  static Future<bool> isTokenExist() async {

    // อ่าน key ที่ชื่อว่า token เข้ามาใช้งานในแอพ
    var token = await storage.read(key: 'token');

    if (token != null && token.isNotEmpty) {
    
      // เก็บ token เอาไว้อ้างอิงจาก class โดยตรง ไม่ต้องอ่านซ้ำๆ 
      userToken = token;
      return true;
    } else {
      return false;
    }
  }

  static Future<String> getToken() async {

    // อ่าน key ที่ชื่อว่า token เข้ามาใช้งานในแอพ และ เก็บ token เอาไว้อ้างอิงจาก class โดยตรง
    var token = await storage.read(key: 'token');
    userToken = token;
    return token;
  }

  static Future<void> saveToken(String token) async {
    // บันทึก token ลง storage 
    await storage.write(key: 'token', value: token);
  }
}

```

## ไฟล์เต็ม

```dart
// token_manager.dart

import 'package:flutter_secure_storage/flutter_secure_storage.dart';

class TokenManager {
  static final storage = new FlutterSecureStorage();

  static String userToken;

  static Future<bool> isTokenExist() async {
    var token = await storage.read(key: 'token');

    if (token != null && token.isNotEmpty) {
      userToken = token;
      return true;
    } else {
      return false;
    }
  }

  static Future<String> getToken() async {
    var token = await storage.read(key: 'token');
    userToken = token;
    return token;
  }

  static Future<void> saveToken(String token) async {
    await storage.write(key: 'token', value: token);
  }
}

```