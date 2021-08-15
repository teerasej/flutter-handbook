
# สร้างหน้า Create News Page

## 1. สร้างไฟล์ lib/create_news_page.dart

ให้เราสร้างไฟล์ชื่อ **create_news_page.dart** ไว้ใน **lib** directory

## 2. สร้าง CreateNewsPage widget 

1. ใช้ code snippet อีกครั้ง โดยการพิมพ์ st 
2. เลือกเป็น Stateful Widget
3. ตั้งชื่อว่า **CreateNewsPage**
4. ทำการเพิ่มคำสั่ง `import 'package:flutter/material.dart';` ไว้ด้านบนของไฟล์
4. กำหนด Scaffold เป็น widget ที่ถูก return ออกจาก Widget นี้

```dart
// lib/create_news_page.dart

import 'package:flutter/material.dart';

class CreateNewsPage extends StatefulWidget {
  const CreateNewsPage({Key? key}) : super(key: key);

  @override
  _CreateNewsPageState createState() => _CreateNewsPageState();
}

class _CreateNewsPageState extends State<CreateNewsPage> {
  @override
  Widget build(BuildContext context) {

    // กำหนด Scaffold เป็น widget ที่ถูก return ออกจาก Widget นี้
    return Scaffold(
      appBar: AppBar(
        title: Text('Create News'),
      ),
    );
  }
}

```

## 3. เปิดหน้า Create News Page จาก Home Page

เราจะเปิดกลับมาที่ไฟล์ **home_page.dart** เพื่อเปลี่ยน Widget ที่ถูก return ออกมาจากคำสึ่ง `Navigator.push()` เป็น `CreateNewsPage()`


```dart
// lib/home_page.dart

//..

Navigator.push(context, MaterialPageRoute(builder: (context) {
    // ให้ builder function คืนค่าออกเป็น CreateNewsPage widget
    return CreateNewsPage();
}));
```

บันทึกไฟล์ และทดสอบใช้งาน

## ไฟล์ lib/home_page.dart ที่ปรับปรุงแล้ว 

```dart
import 'package:flutter/material.dart';
import 'package:news_app/create_news_page.dart';

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
            return CreateNewsPage();
          }));
        },
      ),
    );
  }
}

```