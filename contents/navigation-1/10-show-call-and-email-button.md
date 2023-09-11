
# แสดงปุ่มโทรออก และส่งอีเมลล์

แสดงปุ่มโทรออกในหน้ารายละเอียดผู้ติดต่อ

```dart
import 'package:flutter/material.dart';
import 'package:nextflow_navigation_tab_stack/models/contact_model.dart';

class ContactDetailPage extends StatelessWidget {
  final ContactModel contact;

  ContactDetailPage(this.contact);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(contact.name),
      ),
      // แสดงปุ่มโทรออก
      body: Container(
        padding: EdgeInsets.all(10),
        child: Column(
          children: [
            SizedBox(
              width: double.infinity,
              child: RaisedButton(
                onPressed: () {},
                child: Text('โทร ${contact.tel}'),
              ),
            )
          ],
        ),
      ),
    );
  }
}

```

จากนั้นพิจารณาข้อมูล email ของผู้ติดต่อ ถ้ามี ก็แสดงปุ่มส่ง Email 

```dart
import 'package:flutter/material.dart';
import 'package:nextflow_navigation_tab_stack/models/contact_model.dart';

class ContactDetailPage extends StatelessWidget {
  final ContactModel contact;

  ContactDetailPage(this.contact);

  @override
  Widget build(BuildContext context) {
    
    // สร้างตัวแปรเก็บ Widget
    var emailButton;

    // พิจารณาจากค่า Email ของผู้ติดต่อ
    if (contact.email != null) {
      emailButton = SizedBox(
        width: double.infinity,
        child: RaisedButton(
          onPressed: () {},
          child: Text('ส่ง Email'),
        ),
      );
    } else {
      emailButton = Container();
    }

    return Scaffold(
      appBar: AppBar(
        title: Text(contact.name),
      ),
      body: Container(
        padding: EdgeInsets.all(10),
        child: Column(
          children: [
            SizedBox(
              width: double.infinity,
              child: RaisedButton(
                onPressed: () {},
                child: Text('โทร ${contact.tel}'),
              ),
            ),
            // แสดง Widget ที่เก็บไว้ใน emailButton
            SizedBox(
              height: 10,
            ),
            emailButton
          ],
        ),
      ),
    );
  }
}

```