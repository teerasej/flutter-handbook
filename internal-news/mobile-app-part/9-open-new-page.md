
# ใช้ Navigation เปิดหน้า Page ใหม่ 

1. เปิดไฟล์​ **lib/home_page.dart**
2. ในส่วน class **_HomePageState**
3. เราจะเขียนคำสั่งใน function **onPressed** ของ **FloatingActionButton** widget เพิ่มเติม  

```dart
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

          // ใช้คำสึ่ง Navigator.push
          // กำหนด MaterialPageRoute ในการแสดงหน้าแอพใหม่
          // กำหนด parameter builder: เป็น function ที่ return widget เพื่อเอาไปแสดงบนแอพ
          // ในที่นี้ใช้ข้อความ Text ธรรมดา
          Navigator.push(context, MaterialPageRoute(builder: (context) {
            return Text('Create News');
          }));
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
  const HomePage({Key? key}) : super(key: key);

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

          Navigator.push(context, MaterialPageRoute(builder: (context) {
            return Text('Create News');
          }));
        },
      ),
    );
  }
}

```