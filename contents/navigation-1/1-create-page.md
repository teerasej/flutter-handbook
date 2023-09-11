
# สร้าง Page ที่ต้องการ

เราจำเป็นที่จะต้องสร้าง Widget ที่เป็นตัวแทนของหน้าแอพแต่ละหน้าเตรียมไว้ก่อน

## 1. สร้างหน้า Contact Page

```dart
// lib/pages/contact_page.dart

import 'package:flutter/material.dart';

class ContactPage extends StatefulWidget {
  @override
  _ContactPageState createState() => _ContactPageState();
}

class _ContactPageState extends State<ContactPage> {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```


## 2. สร้างหน้า Setting Page 

```dart
// lib/pages/setting_page.dart

import 'package:flutter/material.dart';

class SettingPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```