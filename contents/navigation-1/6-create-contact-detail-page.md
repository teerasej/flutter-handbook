
# สร้างหน้ารายละเอียดผู้ติดต่อ 

เราจะสร้างหน้าแอพสำหรับแสดงรายละเอียดผู้ติดต่อหลังจากกดเลือกมาจากหน้า Contact Page

```dart
// lib/pages/contact_detail_page.dart

import 'package:flutter/material.dart';
import 'package:nextflow_navigation_tab_stack/models/contact_model.dart';

class ContactDetailPage extends StatelessWidget {

  // property เก็บข้อมูลที่จะถูกส่งผ่านมาจาก หน้า Contact Page
  // จำเป็นต้องประกาศเป็นตัวแปรแบบ final สำหรับ StatelessWidget
  final ContactModel contact;

    // Constructor ที่รับค่ามาจากภายนอก 
  ContactDetailPage(this.contact);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

```