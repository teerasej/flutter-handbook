
# สร้าง Model class สำหรับเป็นตัวแทนข้อมูล

```dart
// lib/models/contact_model.dart

class ContactModel {
  String name;
  String tel;
  String email;

  ContactModel(this.name, this.tel, {this.email});
}

```

นำมาใช้กำหนดข้อมูลใน `contact_page.dart`

```dart
// lib/pages/contact_page.dart

import 'package:flutter/material.dart';
import 'package:nextflow_navigation_tab_stack/models/contact_model.dart';

class ContactPage extends StatefulWidget {
  @override
  _ContactPageState createState() => _ContactPageState();
}

class _ContactPageState extends State<ContactPage> {

    // สร้าง List ของ Contact Model
  List<ContactModel> _contacts = [
    ContactModel('Nextflow Training', '083-071-3373',
        email: 'training@nextflow.in.th'),
    ContactModel('Peter Parker', '083-071-3373'),
    ContactModel('Jonathan Pitch', '086-044-7788'),
    ContactModel('Sid Mier', '085-978-4466'),
    ContactModel('Mary Poppin', '087-689-4478'),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('รายชื่อ'),
      ),
    );
  }
}

```
