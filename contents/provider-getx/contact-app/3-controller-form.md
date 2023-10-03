
# สร้าง controller และใช้ในการเก็บข้อมูลจากแบบฟอร์ม

## 1. สร้าง controller เก็บค่าจากแบบฟอร์ม เวลาพิมพ์

```dart
// lib/pages/new_contact_page/new_contact_controller.dart

import 'package:contact_app/controllers/contact_controller.dart';
import 'package:contact_app/models/contact_model.dart';
import 'package:get/get.dart';

// สร้าง GetxController โดยตั้งชื่อว่า NewContactController จะเอาไว้ใช้กับหน้า New Contact Page 
class NewContactController extends GetxController {

  // สร้างตัวแปร ไว้เก็บชื่อ และอีเมลล์
  String name = '';
  String email = '';

  // สร้าง method ไว้รับค่า name จาก TextField เวลามีการพิมพ์
  void onNameChanged(String value) {
    // เอาค่าที่ได้รับมาเก็บไว้ในตัวแปร
    name = value;
  }

  // สร้าง method ไว้รับค่า email จาก TextField เวลามีการพิมพ์
  void onEmailChanged(String value) {
    // เอาค่าที่ได้รับมาเก็บไว้ในตัวแปร
    email = value;
  }

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

import 'package:contact_app/pages/new_contact_page/new_contact_controller.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';

class NewContactPage extends StatelessWidget {
    // ลบ const ออก
  NewContactPage({super.key});

  // สร้าง instance ของ NewContactController ใส่ลงไปในระบบ GetX และรับตัวแปร controller มาใช้งานกับ widget
  var controller = Get.put(NewContactController());

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
                controller.onNameChanged(value);
              },
            ),
            TextField(
              decoration: InputDecoration(
                labelText: 'Email',
              ),
              // รับค่าจาก TextField ใส่ลงไปใน method controller ถ้าช่อง email มีการพิมพ์หรือลบข้อความ
              onChanged: (value) {
                controller.onEmailChanged(value);
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