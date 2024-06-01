
# สร้างกลไก สร้าง database file และ schema ตอน controller เริ่มทำงาน

ในขั้นตอนนี้เราจะสร้างกลไกในการสร้าง database file และ schema ของ SQLite ในตอนที่ controller เริ่มทำงาน

## 1. เพิ่มกลไกสร้าง database file และ schema เป็น method ใน controller 

```dart
// lib/controllers/contact_controller.dart

import 'package:contact_app/models/contact_model.dart';
import 'package:get/get.dart';

// import package ที่จำเป็น
import 'package:path_provider/path_provider.dart';
import 'package:path/path.dart';
import 'package:sqflite/sqflite.dart';

class ContactController extends GetxController {
  final contacts = <ContactModel>[].obs;

  // สร้าง database object
  Database? _database;

  void addContact(ContactModel contact) {
    contacts.add(contact);
  }

  // สร้าง Method ที่จะมีการสร้างไฟล์ database และ schema ถ้าพบว่าเป็นการสร้างครั้งแรก
  Future<void> _initDatabase() async {

    // ดึง directory ของ application ที่เก็บข้อมูลของแอพ
    final directory = await getApplicationDocumentsDirectory();

    // สร้าง path ที่อยู่ของไฟล์ database โดยการใช้ package path และต่อ path ของ directory กับชื่อไฟล์ database
    final path = join(directory.path, 'contacts.db');

    // สร้าง database file และ schema ถ้ายังไม่มีไฟล์ database นี้
    _database = await openDatabase(
      path,
      version: 1,

      // onCreate จะเป็น function ที่ทำงานเมื่อไฟล์ database ถูกสร้างขึ้น
      onCreate: (db, version) {

        // สร้าง schema ของ database
        return db.execute(
          'CREATE TABLE contacts(name TEXT, email TEXT)',
        );
      },
    );
  }
}

```

## 2.  ตอน controller เริ่มทำงาน ให้เรียก method สร้าง database file และ schema

```dart
import 'package:contact_app/models/contact_model.dart';
import 'package:get/get.dart';
import 'package:path_provider/path_provider.dart';
import 'package:path/path.dart';
import 'package:sqflite/sqflite.dart';

class ContactController extends GetxController {
  final contacts = <ContactModel>[].obs;
  Database? _database;

  // override method onInit ใน GetxController ซึ่งจะทำงานเมื่อ controller ถูกเรียกขึ้นมาใช้งาน
  @override
  void onInit() {
    super.onInit();

    // เรียก method สร้าง database file และ schema
    _initDatabase();
  }

  void addContact(ContactModel contact) {
    contacts.add(contact);
  }

  Future<void> _initDatabase() async {
    final directory = await getApplicationDocumentsDirectory();
    final path = join(directory.path, 'contacts.db');
    _database = await openDatabase(
      path,
      version: 1,
      onCreate: (db, version) {
        return db.execute(
          'CREATE TABLE contacts(name TEXT, email TEXT)',
        );
      },
    );
  }
}

```

## 3. เสร็จแล้ว บันทึกไฟล์

เสร็จแล้ว บันทึกไฟล์