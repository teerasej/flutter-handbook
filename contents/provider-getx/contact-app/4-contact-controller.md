
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

## 2. สร้าง controller แชร์รายชื่อผู้ติดต่อเป็น list และเพิ่ม method สำหรับการเพิ่มผู้ติดต่อลงไปใน list

สร้างไฟล์ `lib/controllers/contact_controller.dart`

```dart
// lib/controllers/contact_controller.dart

import 'package:contact_app/models/contact_model.dart';
import 'package:get/get.dart';

// สร้าง contact controller เพื่อแชร์ข้อมูล contact list ระหว่างหน้า Home และ New contact
class ContactController extends GetxController {

  // สร้างตัวแปร ชนิด list (array) เป็นแบบ observable (obs) เพื่อให้เวลามีการเปลี่ยนแปลงค่าในตัวแปรนี้ จะทำให้ widget Obx() มีการ re-render ตัวเองใหม่
  final contacts = <ContactModel>[].obs;

  // สร้าง method เพื่อให้สามารถเพิ่ม contact ใหม่ลง list ได้
  void addContact(ContactModel contact) {
    contacts.add(contact);
  }
}

```

## 3. ใช้ controller เพื่อเพิ่มผู้ติดต่อใหม่จากฟอร์ม

```dart
// lib/pages/new_contact_page/new_contact_controller.dart

import 'package:contact_app/controllers/contact_controller.dart';
import 'package:contact_app/models/contact_model.dart';
import 'package:get/get.dart';

class NewContactController extends GetxController {
  String name = '';
  String email = '';

  void onNameChanged(String value) {
    name = value;
  }

  void onEmailChanged(String value) {
    email = value;
  }

  void save() {
    print('name: ${name}');
    print('email: ${email}');

    // เรียกใช้ ContactController 
    var contactController = Get.find<ContactController>();

    // เรียกใช้ method addContact ของ ContactController เพื่อเพิ่มชื่อและ email ใหม่ลงไปใน list
    contactController.addContact(
      ContactModel(
        name,
        email,
      ),
    );

    // สั่ง Getx เปิดย้อนกลับไป 1 หน้า
    Get.back();
  }
}

```

## 4. setup contact controller แบบ lazy load

เราจำเป็นต้องทำการเพิ่ม contact controller ที่ Widget ด้านนอกสุดอย่าง MyApp เพื่อที่จะทำให้ widget ต่างๆ สามารถเรียกใช้ contact controller ได้

```dart
// lib/main.dart

import 'package:contact_app/controllers/contact_controller.dart';
import 'package:contact_app/pages/home_page/home_page.dart';
import 'package:contact_app/pages/new_contact_page/new_contact_page.dart';
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
    Get.lazyPut(
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


