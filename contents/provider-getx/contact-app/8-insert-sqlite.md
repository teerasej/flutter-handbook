
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

// import package ที่จำเป็น
import 'package:path_provider/path_provider.dart';
import 'package:path/path.dart';
import 'package:sqflite/sqflite.dart';

import 'package:contact_app/models/contact_model.dart';
import 'package:get/get.dart';

class NewContactController extends GetxController {
  String name = '';
  String email = '';
  var warningMessage = '...'.obs;
  final contacts = <ContactModel>[].obs;

  Database? _database;

  @override
  void onInit() {
    super.onInit();
    _initDatabase();
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

  void save() {
    print('name: ${name}');
    print('email: ${email}');

    if (name.isEmpty || email.isEmpty) {
      warningMessage.value = 'please fill the form';
      print(warningMessage);
    } else {

      var newContact = ContactModel(
        name,
        email,
      );
      contacts.add( newContact);

      _database?.insert(
        'contacts',
        newContact.toJson(),
      );

      Get.back();
    }
  }
}


```