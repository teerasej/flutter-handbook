
# เพิ่มข้อมูลลงใน SQLite database

ในบทนี้เราจะเพิ่มข้อมูลลงใน SQLite database โดยใช้ method `insert` ของ `Database` object ที่เราสร้างไว้ใน controller

## 1. สร้าง Method ใน model class เพื่อใช้แปลงข้อมูลเป็น Map Type หรือ JSON

```dart
// lib/models/contact_model.dart

class ContactModel {
  String name;
  String email;

  ContactModel(
    this.name,
    this.email,
  );
  
  // สร้าง method สำหรับแปลงข้อมูลเป็น Map Type หรือ JSON
  Map<String, dynamic> toJson() {
    return {
      'name': name,
      'email': email,
    };
  }
}

```

## 2. สร้าง method สำหรับเพิ่มข้อมูลลงใน SQLite database

```dart
// lib/controllers/contact_controller.dart

import 'package:contact_app/models/contact_model.dart';
import 'package:get/get.dart';
import 'package:path_provider/path_provider.dart';
import 'package:path/path.dart';
import 'package:sqflite/sqflite.dart';

class ContactController extends GetxController {
  final contacts = <ContactModel>[].obs;
  Database? _database;

  @override
  void onInit() {
    super.onInit();
    _initDatabase();
  }

  void addContact(ContactModel contact) {
    contacts.add(contact);

    // เรียก method insert ของ database object ที่เราสร้างไว้
    _database!.insert('contacts', contact.toJson());
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