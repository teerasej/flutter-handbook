
# สร้างตัวคั่นข้อมูลใน List ด้วย ListView.separated() 


เราจะลองสลับจากการใช้ `ListView.builder()` มาเป็น `ListView.separated()` ที่สามารถกำหนด widget เป็นตัวแบ่ง จาก separatorBuilder ได้

## 1. ทดสอบใช้ ListView.separated()

1. เปิดไฟล์ **lib/home_page.dart**
2. ในส่วน **class _HomePageState**
3. ใน **Scaffold** widget ส่วนของ **body:** 
4. เราจะเปลี่ยนจาก `ListView.builder()` เป็น `ListView.separated()` ซึ่งจะทำให้เรากำหนด ค่าที่ชื่อ `separatorBuilder:` เพิ่มเข้ามาได้ 

```dart
body: ListView.separated(
          itemCount: 10,
          itemBuilder: (BuildContext context, int index) {
            return ListTile(
              title: Text(index.toString()),
            );
          },
          // separatorBuilder รับค่าเป็น function เหมือนกับ itemBuilder แต่ widget ที่ return ออกจาก function นี้จะถูกใช้คั่นระหว่าง widget ของ itemBuilder นั่นเอง
          separatorBuilder: (context, index) {
            return Divider();
          },
        ));
```

บันทึกไฟล์และทดสอบใช้งาน

## ลองเล่น 

ทดสอบกำหนดสีให้กับ Divider widget 

```dart
separatorBuilder: (context, index) {
    return Divider(
        color: Colors.blue
    );
},
```

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
        body: ListView.separated(
          itemCount: 10,
          itemBuilder: (BuildContext context, int index) {
            return ListTile(
              title: Text(index.toString()),
            );
          },
          separatorBuilder: (context, index) {
            return Divider();
          },
        ));
  }
}


```


