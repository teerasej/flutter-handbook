
# สร้างหน้าเพิ่ม contact ใหม่

## 1. สร้างหน้าแบบฟอร์มสำหรับกรอกข้อมูลพนักงานใหม่ 

สร้างไฟล์ `lib/pages/new_contact_page/new_contact_page.dart`

```dart
// lib/pages/new_contact_page/new_contact_page.dart

import 'package:flutter/material.dart';
import 'package:get/utils.dart';

class NewContactPage extends StatelessWidget {
  const NewContactPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('New Contact'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              decoration: InputDecoration(
                labelText: 'Name',
              ),
            ),
            TextField(
              decoration: InputDecoration(
                labelText: 'Email',
              ),
            ),
            const SizedBox(height: 16.0),
            ElevatedButton(
              onPressed: () {},
              child: Text('Save'),
            )
          ],
        ),
      ),
    );
  }
}

```

## 2. เพิ่มหน้าแบบฟอร์มเข้าไปใน app

เปิดไฟล์ `lib/main.dart`

```dart
// lib/main.dart

import 'package:contact_app/pages/home_page/home_page.dart';
import 'package:contact_app/pages/new_contact_page/new_contact_page.dart';
import 'package:flutter/material.dart';
import 'package:get/route_manager.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      title: 'Nextflow Contact App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      initialRoute: '/',
      getPages: [
        GetPage(
          name: '/',
          page: () => HomePage(),
        ),

        // เพิ่ม url และหน้าแบบฟอร์มเพิ่ม contact ใหม่เข้าไปใน app 
        GetPage(
          name: '/new-contact',
          page: () => NewContactPage(),
        ),
      ],
    );
  }
}

```

## 3. ใช้ GetX ในการเปิดหน้าแบบฟอร์ม ตาม URL ที่กำหนด

เปิดไฟล์ `lib/pages/home_page/home_page.dart`

เสร็จแล้วทดสอบเปิดไปหน้าใหม่ดู

```dart
// lib/pages/home_page/home_page.dart

import 'package:flutter/material.dart';
import 'package:get/get.dart';

class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Home'),
        
        // เพิ่มแถบ action ปุ่มใน app bar
        actions: [
          // สร้าง icon button
          IconButton(
            // กำหนด function จะทำงานตอนกดปุ่ม
            onPressed: () {
              // เรียกใช้ Getx ในการเปิดไปยัง URL ที่กำหนดไว้
              Get.toNamed('/new-contact');
            },
            // กำหนดรูปไอคอนของปุ่ม
            icon: const Icon(Icons.add),
          ),
        ],
      ),
    );
  }
}

```