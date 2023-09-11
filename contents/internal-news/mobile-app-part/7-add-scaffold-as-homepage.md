
# กำหนด Scaffold ไว้ในส่วน HomePage

1. เปิดไฟล์​ **lib/home_page.dart**
2. ในส่วน class `_HomePageState` เราจะแก้ไข method `.build()` ให้ return เป็น **Scaffold** widget แทน

```dart
class _HomePageState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {

    // แก้ไขให้ return เป็น Scaffold widget
    return Scaffold(
      appBar: AppBar(
        title: Text('News'),
      ),
    );
  }
}
```

บันทึกไฟล์ และทดสอบใช้งาน

## ไฟล์ lib/home_page.dart ที่ปรับปรุงแล้ว 

```dart
// lib/home_page.dart

import 'package:flutter/material.dart';

class HomePage extends StatefulWidget {
  HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('News'),
      ),
    );
  }
}
```
