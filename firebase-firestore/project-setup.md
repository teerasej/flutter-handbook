# วิธีติดตั้ง Firebase ใช้งานในโปรเจค Google Flutter

## 1. สมัคร firebase account 
   ที่ https://firebase.google.com/


## 2. Setup firebase CLI 

เปิด terminal และรันคำสั่ง

```bash
curl -sL https://firebase.tools | bash
```

เสร็จแล้วรันคำสั่งด้านล่างเพื่อ sign in เข้าใช้งานด้วยบัญชี firebase ของเราเอง

```
firebase login
```

## 3. ติดตั้ง flutterfire_cli โดยการรันคำสั่งด้านล่างจากที่ไหนก็ได้ 

```
dart pub global activate flutterfire_cli
```

## 4. setup Firebase profile ในโปรเจค

เปิดโฟลเดอร์โปรเจค Flutter ขึ้นมาใน Terminal และรันคำสั่งด้านล่าง 

```
flutterfire configure
```

จากนั้นเลือกรายชื่อโปรเจค หรือสร้างใหม่ (อย่าลืมว่า account ฟรีสร้างได้จำนวนจำกัด)

## 5. ติดตั้ง Package firebase_core

```
flutter pub add firebase_core
```

เสร็จแล้วรันคำสั่ง เพื่อ sync ข้อมูลในโปรเจคกับที่อยู่บน Firebase 

> คำสั่งนี้จะทำให้แน่ใจว่าข้อมูลทั้งหมด sync กับตัวโปรเจคที่อยู่บน firebase ดังนั้นอย่าแปลกใจที่การ setup ต่างๆ จะจบลงที่คำสั่งนี้

```
flutterfire configure
```

## 6. Setup Firebase ให้พร้อมในงานใน Dart 

ปรับแต่ง และเพิ่มโค้ดด้านล่างลงไปใน main function

```dart
// lib/main.dart 

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );

  runApp(const MyApp());
}
```

