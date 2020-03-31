
# Build from Scratch 1

## 1. ล้างโค้ดทั้งหมด

ล้างโค้ดในไฟล์ `lib/main.dart` ทั้งหมด
จากนั้นสร้างเขียนโค้ดใหม่เป็น

```dart
void main() {
	print('hello...');
}
```

## 2. comment test code

เปิดไฟล์ `test/widget_test.dart` และ comment โค้ดทั้งหมดไว้ก่อน

## 3. ทดสอบแอพ

จากนั้นรันทดสอบแอพพลิเคชั่น โดยกดปุ่ม **F5**

สำหรับ Visual Studio Code ให้เปิดดูในส่วนของ Debug Console 
หลังจากกระบวนการทุกอย่างเสร็จสิ้น เราควรจะเห็นข้อความว่า 

flutter: hello...

## 4. เริ่มสร้าง Material App

1. ลบ `print('hello...');` ออกจาก `main()` 
2. เขียนคำสั่ง `import 'package:flutter/material.dart';` เพื่อเรียกใช้คำส่ังที่จำเป็นในการสร้าง Material App
3. ใน `main()` เขียนคำสั่ง `runApp()`

```dart
runApp(MaterialApp(
    title: 'My Flutter'
));
```

ไฟล์สำเร็จจะเป็นแบบด้านล่างนี้ 

```dart
import 'package:flutter/material.dart'

void main() {
	runApp(MaterialApp(
        title: 'My Flutter'
    ));
}
```

เจ้านี่คือตัวโครงแอพพลิเคชั่นของเรา สังเกตผลว่าเกิดอะไรขึ้นถ้าเรารันทดสอบแอพตอนนี้ 

## 5. สร้างหน้าแรกของแอพพลิเคชั่น 

ให้เพิ่มส่วนของ `:home` เข้าไปใน `MeterialApp()`

```dart
runApp(MaterialApp(
    title: 'My Flutter',
    home: Scaffold(
      appBar: AppBar(
        title: Text('OK'),
      ),
      body: Text('hello'),
    ),
  ));
```

จากนั้นให้รันทดสอบแอพพลิเคชั่น 

สังเกตว่า Widget ที่ช่ื่อ `Scaffold()` เป็นโครงหน้าของแอพพลิเคชั่น โดยประกอบไปด้วย Widget ดังนี้
1. `AppBar()` กำหนดให้กับ `appBar:`
2. `Text()` ที่กำหนดให้กับ `body:`

## 6. กำหนด Theme สีให้ App

ให้เพิ่ม `theme:` เข้าไปใน `MaterialApp()` 

runApp(MaterialApp(
    ...
    theme: ThemeData(
      primarySwatch: Colors.red
    ),
	...

รันทดสอบแอพพลิเคชั่น และให้สังเกตการเปลี่ยนแปลงของแอพพลิเคชั่น

Property ชื่อ `theme:` ของ `MaterialApp()` นั้นรับค่า `ThemeData()` ซึ่งเราสามารถกำหนดค่าสีเพื่อใช้งานกับทั้งแอพพลิเคชั่นได้ 

`Colors` เป็นคลาสที่มีค่าสีต่างๆ ให้เราเลือกนั่นเอง

> ลองกำหนดค่าสีใหม่ๆ และทดสอบแอพพลิเคชั่นดู ให้สังเกตการเปลี่ยนแปลงของแอพพลิเคชั่น ตามสีที่กำหนด

จากนั้นทดสอบ ค่า theme อีกแบบหนึ่ง

runApp(MaterialApp(
    ...
    theme: ThemeData(
      brightness: Brightness.dark
    ),
	...

รันทดสอบแอพพลิเคชั่น ให้สังเกตการเปลี่ยนแปลงของแอพพลิเคชั่น 

นี่คือ Dark theme นั่นเอง (ใช้ได้ใน Android 10 แล้วนะ)