
# แสดงปุ่มลอยสำหรับเปิดหน้าสร้างข่าวใหม่

1. เปิดไฟล์​ **lib/home_page.dart**
2. ในส่วน class **_HomePageState**
   
- เพิ่มปุ่มลอย FloatingActionButton widget เข้าไปใน Scaffold
- กำหนด Icon widget ให้กับปุ่มลอย
- กำหนด function ที่จะทำงานเมื่อมีการกดปุ่ม

```dart
class _HomePageState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('News'),
      ),
      // เพิ่มปุ่มลอย FloatingActionButton widget เข้าไปใน Scaffold
      floatingActionButton: FloatingActionButton(
        // กำหนด Icon widget ให้กับปุ่มลอย
        child: Icon(Icons.add),
        // กำหนด function ที่จะทำงานเมื่อมีการกดปุ่ม
        onPressed: () {
          // แสดงข้อความขึ้นมาใน console
          print('click');
        },
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
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.add),
        onPressed: () {
          print('click');
        },
      ),
    );
  }
}
```