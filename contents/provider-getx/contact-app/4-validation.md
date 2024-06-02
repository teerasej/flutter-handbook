
# แสดงข้อความเตือนเมื่อไม่ได้กรอกข้อมูลในแบบฟอร์ม

## 1. สร้างตัวแปรเก็บข้อความเตือน

```dart
// lib/controllers/contact_controller.dart

import 'package:get/get.dart';

class ContactController extends GetxController {
  var name = "";
  var email = "";

  // สร้างตัวแปรเก็บข้อความเตือน
  var warningMessage = "";

  save() {
    print('ชื่อ: $name');
    print('email: $email');

    if (name.isEmpty || email.isEmpty) {

      // ถ้าไม่มีการพิมพ์ข้อมูลในช่อง name และ email กำหนดข้อความเตือนว่า Please fill the form
      warningMessage = 'โปรดกรอกข้อมูลให้ครบ';

    } else {
      Get.back();
    }
  }
}
```

## 2. แสดงข้อความเตือนในหน้า new contact

```dart
// lib/controllers/new_contact_page.dart

import 'package:contact_app/controllers/contact_controller.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';

class NewContactPage extends StatelessWidget {
  NewContactPage({super.key});

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
              onChanged: (value) {
                controller.name = value;
              },
              decoration: InputDecoration(
                labelText: 'Name',
              ),
            ),
            TextField(
              decoration: InputDecoration(
                labelText: 'Email',
              ),
              onChanged: (value) {
                controller.email = value;
              },
            ),
            const SizedBox(height: 16.0),
            ElevatedButton(
              onPressed: () {
                controller.save();
              },
              child: Text('Save'),
            ),

            // แสดงข้อความเตือน โดยใช้ตัวแปร warningMessage จาก controller
            const SizedBox(height: 16.0),
            Text('${controller.warningMessage}'),
          ],
        ),
      ),
    );
  }
}
```

> จะสังเกตว่า ถ้าเราพิมพ์ข้อมูลในช่อง name และ email แล้วกดปุ่ม save จะไม่มีข้อความเตือนแสดง แต่ถ้าไม่ได้พิมพ์ข้อมูลในช่อง name และ email จะมีข้อความเตือนแสดงขึ้นมา
> แต่ตอนนี้ไม่เห็นมีข้อความเตือนขึ้นมาเลยยยยยยยย!!!

## 3. ใช้ตัวแปร observable ในการเก็บข้อความเตือน

```dart
// lib/controllers/contact_controller.dart
import 'package:get/get.dart';

class ContactController extends GetxController {
  var name = "";
  var email = "";

  // เปลี่ยนมาใช้ตัวแปร observable ในการเก็บข้อความเตือน
  var warningMessage = "".obs;

  save() {
    print('ชื่อ: $name');
    print('email: $email');

    if (name.isEmpty || email.isEmpty) {
      
      // ถ้าไม่มีการพิมพ์ข้อมูลในช่อง name และ email กำหนดข้อความเตือนว่า Please fill the form
      // สามารถกำหนดค่าให้กับตัวแปร observable ได้โดยใช้ .value
      warningMessage.value = 'โปรดกรอกข้อมูลให้ครบ';

      // สามารถกำหนดค่าให้กับตัวแปร observable ได้โดยใช้รูปแบบของ function 
      warningMessage('โปรดกรอกข้อมูลให้ครบ');

    } else {
      Get.back();
    }
  }
}

```

## 4. แสดงข้อความเตือน โดยใช้ค่า value ของตัวแปร observable ในหน้า new contact

```dart
// lib/pages/new_contact_page/new_contact_page.dart

import 'package:contact_app/controllers/contact_controller.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';

class NewContactPage extends StatelessWidget {
  NewContactPage({super.key});

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
              onChanged: (value) {
                controller.name = value;
              },
              decoration: InputDecoration(
                labelText: 'Name',
              ),
            ),
            TextField(
              decoration: InputDecoration(
                labelText: 'Email',
              ),
              onChanged: (value) {
                controller.email = value;
              },
            ),
            const SizedBox(height: 16.0),
            ElevatedButton(
              onPressed: () {
                controller.save();
              },
              child: Text('Save'),
            ),
            const SizedBox(height: 16.0),

            // ใช้ Obx widget ของ GetX ในการแสดงข้อความเตือน โดยใช้การเปลี่ยนแปลงค่าในตัวแปร observable ของ controller เป็นตัว trigger ให้เปลี่ยนแปลง UI
            Obx(() {
              
              // ปรับมาใช้ค่า .value ของตัวแปร observable ในการแสดงข้อความเตือน
              return Text(
                '${controller.warningMessage.value}',
                style: TextStyle(
                  color: Colors.red,
                ),
              );
            })
          ],
        ),
      ),
    );
  }
}
```
