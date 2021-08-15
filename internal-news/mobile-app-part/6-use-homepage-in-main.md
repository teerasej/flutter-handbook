
# นำ HomePage Widget มาใช้กับ MaterialApp widget ในไฟล์ main.dart 

## 1. แทนที่ Text widget ด้วย HomePage widget 

เปิดมาที่ไฟล์ main.dart ที่เราสร้าง MyApp widget ไว้ 
ใน class **MyApp** > method **build()** เราจะแก้ไขค่า **home** ของ **MaterialApp** widget เป็นชื่อของ HomePage widget

สังเกตว่าพอเราพิมพ์คำว่า **HomePage** จะมีเมนูตัวช่วยแสดงขึ้นมา **ให้เลือกใช้เมนูตัวช่วย** 

<img width="824" alt="2021-08-15_18-34-05" src="https://user-images.githubusercontent.com/85179/129478135-28de288c-f795-4fd1-8f65-ccd5687a73ca.png">


สังเกตว่าหลังจากเลือก `HomePage()` widget จากเมนูตัวช่วย จะมีการเขียน import ไฟล์ **lib/home_page.dart** ด้านบนอัตโนมัติ

<img width="672" alt="2021-08-15_19-16-21" src="https://user-images.githubusercontent.com/85179/129478242-647b8f57-7480-4aba-b3ff-a1beb9cf1485.png">



- การเรียกใช้ HomePage widget ที่ไฟล์อื่นนอกเหนือจากไฟล์ home_page.dart ต้องมีการเขียนคำสั่ง import ไฟล์ home_page.dart ให้ถูกต้องด้วย 
- ถ้าไม่มีเมนูตัวช่วย ส่วนนี้เราต้องเขียนอ้างอิงที่อยู่ด้วยตัวเอง


**การใช้เมนูตัวช่วยจะทำให้การเขียนโค้ดสะดวกมากขึ้น และผิดพลาดน้อยลง หัดใช้ให้คล่องนะครับ**


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
