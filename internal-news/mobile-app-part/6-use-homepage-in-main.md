
# นำ HomePage Widget มาใช้กับ MaterialApp widget ในไฟล์ main.dart 

## 1. แทนที่ Text widget ด้วย HomePage widget 

เปิดมาที่ไฟล์ main.dart ที่เราสร้าง MyApp widget ไว้ 
ใน class **MyApp** > method **build()** เราจะแก้ไขค่า **home** ของ **MaterialApp** widget เป็นชื่อของ HomePage widget



สังเกตว่าพอเราพิมพ์คำว่า HomePage จะมีเมนูตัวช่วยแสดงขึ้นมา ให้เลือกใช้เมนูตัวช่วย 


สังเกตว่าหลังจากเลือก HomePage() widget จากเมนูตัวช่วย จะมีการเขียน import ไฟล์ lib/home_page.dart ด้านบนอัตโนมัติ

- การเรียกใช้ HomePage widget ที่ไฟล์อื่นนอกเหนือจากไฟล์ home_page.dart ต้องมีการเขียนคำสั่ง import ไฟล์ home_page.dart ให้ถูกต้องด้วย 
- การใช้เมนูตัวช่วยจะทำให้การเขียนโค้ดสะดวกมากขึ้น และผิดพลาดน้อยลง หัดใช้ให้คล่องนะครับ

```dart
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'News App',
      home: Text('oh, hello'),
    );
  }
}
```

## ไฟล์ lib/main.dart ที่ปรับปรุงเสร็จแล้ว

```dart
// lib/main.dart

import 'package:flutter/material.dart';
import 'package:news_app/home_page.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'News App',
      home: HomePage(),
    );
  }
}

```