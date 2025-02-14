
# การติดตั้ง และ setup GetX Provider

## 1. ติดตั้ง GetX package 

[รันคำสั่งใน terminal หรือใช้การเพิ่มชื่อ package ตามรายละเอียดในลิ้งค์](https://pub.dev/packages/get/install)

## 2. สลับมาใช้งาน GetMaterialApp แทน MaterialApp

เปลี่ยนจาก `MaterialApp` เป็น `GetMaterialApp` ในไฟล์ `lib/main.dart`

```dart
// lib/main.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';
import 'package:nextflow_chatgpt/pages/chat/chat_page.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {

    // แก้ไขจาก MaterialApp เป็น GetMaterialApp
    return GetMaterialApp(
      title: 'Nextflow ChatGPT',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        useMaterial3: true,
      ),
      // ลบ home: และเพิ่ม getPages: ดังนี้
      getPages: [
        // เรียกใช้งาน ChatPage โดยกำหนดชื่อ page route เป็น '/'
        GetPage(name: '/', page: () => ChatPage()),
      ],
    );
  }
}

```

บันทึกไฟล์และรีเฟรชหน้าจอของแอพพลิเคชั่น จะเห็นว่าแอพพลิเคชั่นไม่มีการเปลี่ยนแปลงอะไรเลย แต่เราได้เปลี่ยนจาก `MaterialApp` เป็น `GetMaterialApp` แล้ว