
# กำหนดหน้าตาของ Contact Page และ Setting Page

```dart
// lib/pages/setting_page.dart
import 'package:flutter/material.dart';

class SettingPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('ตั้งค่า'),
      ),
    );
  }
}

```


```dart
//lib/pages/contact_page.dart

import 'package:flutter/material.dart';

class ContactPage extends StatefulWidget {
  @override
  _ContactPageState createState() => _ContactPageState();
}

class _ContactPageState extends State<ContactPage> {
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