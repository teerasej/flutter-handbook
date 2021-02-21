# แสดงรายละเอียดผู้ติดต่อ

```dart
// lib/pages/contact_detail_page.dart

import 'package:flutter/material.dart';
import 'package:nextflow_navigation_tab_stack/models/contact_model.dart';

class ContactDetailPage extends StatelessWidget {
  final ContactModel contact;

  ContactDetailPage(this.contact);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
          // แสดงชื่อผู้ติดต่อใน App Bar
        title: Text(contact.name),
      ),
    );
  }
}

```