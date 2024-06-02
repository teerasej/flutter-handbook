
# แสดงรายชื่อ contact ผู้ติดต่อในหน้า home

มี 5 ขั้นตอน

## 1. สร้าง model สำหรับข้อมูลผู้ติดต่อ

สร้างไฟล์ `lib/models/contact_model.dart`

```dart
// lib/models/contact_model.dart

class ContactModel {
  String name;
  String email;

  ContactModel(
    this.name,
    this.email,
  );
}
```

## 2. เพิ่ม List ของ contact ใน controller

เปิดไฟล์ `lib/controllers/contact_controller.dart`

```dart
import 'package:contact_app/models/contact_model.dart';
import 'package:get/get.dart';

class ContactController extends GetxController {
  String name = '';
  String email = '';
  var warningMessage = '...'.obs;

  // สร้าง List ของ ContactModel ที่เป็น observable ด้วย .obs
  final contacts = <ContactModel>[].obs;

  void save() {
    print('name: ${name}');
    print('email: ${email}');

    if (name.isEmpty || email.isEmpty) {
      warningMessage.value = 'please fill the form';
      print(warningMessage);
    } else {

      // เพิ่มข้อมูลใหม่ลงใน List ของ contacts
      var newContact = ContactModel(
        name,
        email,
      );
      contacts.add( newContact);
      
      Get.back();
    }
  }
}


```

## 3. setup new contract controller ในที่ใหมด้านนอกสุด

เราจำเป็นต้องทำการเพิ่ม contact controller ที่ Widget ด้านนอกสุดอย่าง MyApp เพื่อที่จะทำให้ widget ต่างๆ สามารถเรียกใช้ contact controller ได้

```dart
// lib/main.dart

import 'package:contact_app/controllers/contact_controller.dart';
import 'package:contact_app/pages/home_page/home_page.dart';
import 'package:contact_app/controllers/contact_page.dart';
import 'package:flutter/material.dart';
import 'package:get/route_manager.dart';
import 'package:get/get.dart';

void main() {
    // ลบ const
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
    // ลบ const
  MyApp({super.key});

  @override
  Widget build(BuildContext context) {

    // ทำการเพิ่ม Contact Controller ลงไปใน GetX 
    Get.put(
      () {
        return ContactController();
      },
    );

    return GetMaterialApp(
      title: 'Nextflow Contact App',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
        appBarTheme: const AppBarTheme(
          backgroundColor: Colors.blue,
          foregroundColor: Colors.white,
        ),
      ),
      initialRoute: '/',
      getPages: [
        GetPage(
          name: '/',
          page: () => HomePage(),
        ),
        GetPage(
          name: '/new-contact',
          page: () => NewContactPage(),
        ),
      ],
    );
  }
}

```

## 4. Update หน้า New Contact Page ให้ใช้วิธีการหา (Get.find) ตัว controller จาก GetX

```dart
import 'package:contact_app/controllers/contact_controller.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';

class NewContactPage extends StatelessWidget {
  NewContactPage({super.key});

  // ใช้ Get.find ในการหา controller จาก GetX
  var controller = Get.find<ContactController>();

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
              onChanged: (String value) {
                controller.name = value;
              },
            ),
            TextField(
              decoration: InputDecoration(
                labelText: 'Email',
              ),
              onChanged: (String value) {
                controller.email = value;
              },
            ),
            const SizedBox(height: 16.0),
            ElevatedButton(
              onPressed: () {
                controller.save();
                Get.back();
              },
              child: Text('Save'),
            ),
            Obx(() {
              return Text(controller.warningMessage.value);
            })
          ],
        ),
      ),
    );
  }
}
```

## 5. เรียกใช้ list มาสร้างรายการผู้ติดต่อ

เสร็จขั้นตอนนี้แล้ว ทดสอบการทำงานดู

```dart
// lib/pages/home_page/home_page.dart

import 'package:contact_app/controllers/contact_controller.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';

class HomePage extends StatelessWidget {
  HomePage({super.key});

  // เรียกใช้ ContactController จากระบบ GetX
  var contactController = Get.find<ContactController>();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Home'),
        actions: [
          IconButton(
            onPressed: () {
              Get.toNamed('/new-contact');
            },
            icon: const Icon(Icons.add),
          ),
        ],
      ),
      // เรียกใช้ Obx widget ของ GetX
      // ถ้าใน Obx widget มีการเรียกใช้ตัวแปร observable (obs) ไหนของ controller แล้วมีการแก้ไข หรือแทนที่ค่าเดิมของตัวแปรนั้น 
      // จะทำให้ widget obx rebuild ตัวเองใหม่ ทำให้มีการดึงค่าใหม่ของตัวแปนมาใช้เสมอ
      body: Obx(
        () => ListView.builder(

          // แสดงจำนวนของ item ใน contact list ของ controller
          itemCount: contactController.contacts.length,

          // itemBuilder จะกำหนด function ที่จะส่งเลข index เข้ามาให้เรา เอามาใช้อ้างอิงในการสร้าง list item ขึ้นมาเป็น UI 
          itemBuilder: (context, index) {

            // เอาเลข index มาดึงข้อมูลจาก contact list ของ controller
            var contact = contactController.contacts[index];

            // เอาข้อมูล contact มาแสดง
            return ListTile(
              title: Text(contact.name),
              subtitle: Text(contact.email),
            );
          },
        ),
      ),
    );
  }
}

```


