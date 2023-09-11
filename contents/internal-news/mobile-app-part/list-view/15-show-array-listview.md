
# แสดงชุดข้อมูลใน List

ข้อมูลเป็นชุดส่วนใหญ่ในแอพ เช่น 

- สินค้าใหม่ที่มาในเดือนนี้
- คูปองลดราคาทั้งหมด
- อาหารทั้งหมดที่สั่งได้ 

ทั้งนี้ จะอยู่ในรูปแบบข้อมูลที่เรียกว่า List ซึ่งข่าวที่เราจะได้รับจาก Web API จะส่งมาในรูปแบบ List เช่นเดียวกัน 

มาลองสร้าง List ข้อมูลง่ายๆ แสดงใน ListView widget กันก่อน

## 1. สร้างตัวแปรแบบ List ไว้ใน HomePage widget

1. เปิดไฟล์ **lib/home_page.dart**
2. ในส่วน **class _HomePageState**
3. เราจะสร้างตัวแปรชื่อ newsItems แบบ List เอาไว้เก็บข้อความอย่างเดียวไว้ที่นี่

```dart
// สังเกตว่าประกาศเอาไว้ใน class _HomePageState แต่ไม่ได้เอาไว้ใน method build()
class _HomePageState extends State<HomePage> {

  // ประกาศตัวแปรชื่อ newsItems แบบ List
  // List<String> เป็นการกำหนดว่าข้อมูลใน List นี้จะเป็นแบบ String เท่านั้น
  // ในที่นี้กำหนดข้อความ 3 ข้อความไว้ใน List   
  List<String> newsItems = ['abc', 'def', 'ghi'];

  //...
```

## 2. ปรับให้ ListView ใช้ข้อมูลจาก newsItem

1. ลงมาที่ **ListView.separated()**
2. ปรับให้ค่า `itemCount` ของ ListView ใช้จำนวนข้อมูลจากตัวแปร **newsItem** ผ่าน `newsItem.length`
3. ใน itemBuilder เราปรับให้ข้อความใน ListTile ดึงค่าออกจาก newsItem โดยใช้เลข index

```dart
body: ListView.separated(
          // ปรับให้ค่า `itemCount` ของ ListView ใช้จำนวนข้อมูลจากตัวแปร **newsItem** ผ่าน `newsItem.length`
          itemCount: newsItems.length,
          itemBuilder: (BuildContext context, int index) {
            return ListTile(

              // เราปรับให้ข้อความใน ListTile ดึงค่าออกจาก newsItem โดยใช้เลข index ระบุลำดับของข้อมูลที่เก็บไว้
              title: Text(newsItems[index]),
            );
          },
          separatorBuilder: (context, index) {
            return Divider();
          },
        ));
```

บันทึกไฟล์ และทดสอบ

## ไฟล์ lib/home_page.dart ที่ปรับปรุงแล้ว 

```dart
// lib/home_page.dart

import 'package:flutter/material.dart';
import 'create_news_page.dart';

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  List<String> newsItems = ['abc', 'def', 'ghi'];

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
          itemCount: newsItems.length,
          itemBuilder: (BuildContext context, int index) {
            return ListTile(
              title: Text(newsItems[index]),
            );
          },
          separatorBuilder: (context, index) {
            return Divider();
          },
        ));
  }
}


```