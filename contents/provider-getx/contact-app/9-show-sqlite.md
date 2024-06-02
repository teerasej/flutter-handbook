
# แสดงข้อมูลจาก SQLite database ในหน้า home

ในบทนี้เราจะแสดงข้อมูลจาก SQLite database ในหน้า home โดยใช้ `ListView.builder` และ `Obx` widget ของ GetX

## 1. สร้าง method ใน controller สำหรับดึงข้อมูลจาก SQLite database

```dart
// lib/controllers/contact_controller.dart
import 'package:contact_app/models/contact_model.dart';
import 'package:get/get.dart';
import 'package:path_provider/path_provider.dart';
import 'package:path/path.dart';
import 'package:sqflite/sqflite.dart';

class ContactController extends GetxController {
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
  
  // สร้าง method สำหรับดึงข้อมูลจาก SQLite database
  Future<void> loadContacts() async {

    // ถ้าเช็คแล้วว่า ระบบยังไม่ได้สร้าง database object ให้เรียก method _initDatabase
    if (_database == null) await _initDatabase();

    // ดึงข้อมูลจาก SQLite database และเก็บค่าที่ได้ไว้ในตัวแปร dbContacts
    final dbContacts = await _database!.query('contacts');
    // สามารถเขียนแบบนี้ได้เช่นกัน 
    // final dbContacts = await _database!.rawQuery('SELECT * FROM contacts');

    // แปลงข้อมูลจาก SQLite database ให้เป็น List<ContactModel> แล้วเก็บไว้ในตัวแปร contacts
    // ขั้นตอนนี้ จะทำให้ข้อมูลที่อยู่ใน SQLite database ถูกแสดงในหน้า home ผ่าน Obx widget
    contacts.value = dbContacts.map((dbContact) {
      return ContactModel(
        dbContact['name'] as String,
        dbContact['email'] as String,
      );
    }).toList();
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

## 2. เรียกแสดงข้อมูลจาก SQLite database ในหน้า home ก่อนมีการ return ของ `Scaffold`

```dart
// lib/pages/home_page.dart
import 'package:contact_app/controllers/contact_controller.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';

class HomePage extends StatelessWidget {
  HomePage({super.key});

  var contactController = Get.find<ContactController>();

  @override
  Widget build(BuildContext context) {

    // เรียก method loadContacts ที่เราสร้างไว้ใน controller เพื่อดึงข้อมูลจาก SQLite database
    contactController.loadContacts();

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
      // show listview of contacts from contact controller with Obx widget
      body: Obx(
        () => ListView.builder(
          itemCount: contactController.contacts.length,
          itemBuilder: (context, index) {
            var contact = contactController.contacts[index];
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
