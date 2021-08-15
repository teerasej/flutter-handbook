
# ใช้ ListView แสดงตัวเลขในรายการ

เรากลับมาที่หน้า Home Page เพื่อสร้างตัวแสดงรายการข่าว ในที่นี้เราจะใช้ ListView widget

## 1. ทดสอบใช้ ListView.builder()

1. เปิดไฟล์ **lib/home_page.dart**
2. ในส่วน **class _HomePageState**
3. ใน **Scaffold** widget เราจะกำหนด **ListView** widget ลงไปในส่วน `body:` เพื่อให้แสดงขึ้นมาบนหน้า Home Page

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

            Navigator.push(context, MaterialPageRoute(builder: (context) {
              return CreateNewsPage();
            }));
          },
        ),

        // กำหนด ListView.builder() 
        // .builder() function จะรับค่า 2 ค่า
        //  itemCount: จำนวนที่ builder จะนับ โดยเริ่มจาก 0 จนถึงค่าที่กำหนดใน itemCount
        //  itemBuilder: เป็น function ที่รับค่า index (ค่าที่นับไล่จาก 0 ของ ListView เวลาทำงาน) function itemBuilder นี้ต้อง return widget ออกไป เพื่อให้ ListView เอาไปแสดงเป็นรายการบนหน้าแอพ
        body: ListView.builder(
          itemCount: 10,
          itemBuilder: (BuildContext context, int index) {

            // ตัวอย่างนี้ เอาค่า index มาแสดงเป็นข้อความ ผ่าน Text widget
            return Text(index.toString());
          },
        ));
  }
}
```

บันทึกไฟล์และทดสอบใช้งาน

## 2. ใช้ ListTile widget 

ในที่นี้เราสามารถใช้ ListTile widget สร้าง layout สวยๆ แทนการ custom เองได้ 

```dart
body: ListView.builder(
          itemCount: 10,
          itemBuilder: (BuildContext context, int index) {

            // เปลี่ยนจาก Text widget ธรรมดา เป็น ListTile widget ซึ่งมีค่าต่างๆ ให้ใช้จัด layout มากกว่า
            return ListTile(
              title: Text(index.toString()),
            );
          },
        ));
```

บันทึกไฟล์และทดสอบใช้งาน

## ไฟล์ lib/home_page.dart ที่ปรับปรุงแล้ว 

```dart
// lib/home_page.dart

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
        body: ListView.builder(
          itemCount: 10,
          itemBuilder: (BuildContext context, int index) {
            return ListTile(
              title: Text(index.toString()),
            );
          },
        ));
  }
}


```


