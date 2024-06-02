
# สร้าง controller และใช้ในการเก็บข้อมูลจากแบบฟอร์ม

## 1. สร้าง controller เก็บค่าจากแบบฟอร์ม เวลาพิมพ์

```dart
// lib/controllers/new_contact_controller.dart

import 'package:contact_app/controllers/contact_controller.dart';
import 'package:contact_app/models/contact_model.dart';
import 'package:get/get.dart';

// สร้าง GetxController โดยตั้งชื่อว่า ContactController จะเอาไว้ใช้กับหน้า New Contact Page 
class ContactController extends GetxController {

  // สร้างตัวแปร ไว้เก็บชื่อ และอีเมลล์
  String name = '';
  String email = '';

  // สร้าง method ไว้แสดงชื่อ และ email ที่เก็บในตัวแปร
  void save() {
    print('name: ${name}');
    print('email: ${email}');
  }
}

```

## 2. setup และเรียกใช้ controller

```dart 
// lib/pages/new_contact_page/new_contact_page.dart

import 'package:contact_app/controllers/new_contact_controller.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';

class NewContactPage extends StatelessWidget {
    // ลบ const ออก
  NewContactPage({super.key});

  // สร้าง instance ของ ContactController ใส่ลงไปในระบบ GetX และรับตัวแปร controller มาใช้งานกับ widget
  var controller = Get.put(ContactController());

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
              // รับค่าจาก TextField ใส่ลงไปใน method controller ถ้าช่อง name มีการพิมพ์หรือลบข้อความ
              onChanged: (value) {
                controller.name = value ?? '';
              },
            ),
            TextField(
              decoration: InputDecoration(
                labelText: 'Email',
              ),
              // รับค่าจาก TextField ใส่ลงไปใน method controller ถ้าช่อง email มีการพิมพ์หรือลบข้อความ
              onChanged: (value) {
                controller.email = value ?? '';
              },
            ),
            const SizedBox(height: 16.0),
            ElevatedButton(
              onPressed: () {

                // เรียกใช้งาน method save() ของ controller เพื่อยืนยันค่าที่เก็บไว้ในตัวแปร name และ email
                controller.save();
              },
              child: Text('Save'),
            )
          ],
        ),
      ),
    );
  }
}

```

## 3. เพิ่มเงื่อนไขการบันทึกข้อมูล

ว่าต้องมีการพิมพ์ข้อมูลในช่อง name และ email ก่อน ถึงจะทำการบันทึกข้อมูลได้


```dart
// lib/controllers/contact_controller.dart

import 'package:get/get.dart';

class ContactController extends GetxController {
  String name = "";
  String email = "";

  save() {
    print('name: $name');
    print('email: $email');

    // ใช้ if else เพื่อเช็คว่า ช่อง name และ email มีการพิมพ์ข้อมูลหรือไม่
    if (name.isEmpty || email.isEmpty) {

      // ถ้าไม่มีการพิมพ์ข้อมูลในช่อง name และ email ให้แสดงข้อความว่า Please fill the form
      print('Please fill the form');

    } else {

      // ถ้ามีการพิมพ์ข้อมูลในช่อง name และ email ให้เรียกใช้งาน GetX ในการเปลี่ยนหน้าไปยังหน้า Home Page
      Get.back();
    }
  }
}
```