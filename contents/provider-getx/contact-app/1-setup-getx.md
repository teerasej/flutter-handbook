
# ติดตั้ง GetX และ setup

## 1. ติดตั้ง GetX package 

[รันคำสั่งใน terminal หรือใช้การเพิ่มชื่อ package ตามรายละเอียดในลิ้งค์](https://pub.dev/packages/get/install)

## 2. สลับมาใช้ `GetMaterial` widget

เปิดไฟล์ `lib/main.dart`

```dart
import 'package:contact_app/pages/home_page/home_page.dart';
import 'package:flutter/material.dart';
import 'package:get/route_manager.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {

    // เปลี่ยนจาก MaterialApp เป็น GetMaterialApp
    return GetMaterialApp(
      title: 'Nextflow Contact App',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,

        // กำหนด theme สีพื้นหลัง และสีตัวอักษรของ App Bar
        appBarTheme: const AppBarTheme(
          backgroundColor: Colors.blue,
          foregroundColor: Colors.white,
        ),
      ),

      // กำหนด url เริ่มต้นของแอพ
      initialRoute: '/',

      // กำหนด URL และ widget ต้องการแสดง ด้วย GetPage
      getPages: [
        GetPage(
          name: '/',
          page: () => HomePage(),
        ),
      ],
    );
  }
}

```