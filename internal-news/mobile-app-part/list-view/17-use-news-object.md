
# นำ class NewsData มาใช้กำหนดข้อมูลข่าว 


มาลองสร้าง List ที่คราวนี้กำหนดเป็น  แสดงใน ListView widget กันก่อน

## 1. สร้างตัวแปร List ไว้ใน HomePage widget

1. เปิดไฟล์ **lib/home_page.dart**
2. ในส่วน **class _HomePageState**
3. เราจะสร้างตัวแปรชื่อ newsItems แบบ List ซึ่งคราวนี้จะเป็นการกำหนดเก็บข้อมูลแบบ Class NewsData 

```dart
// เนื่องจากเราประกาศ NewsData ไว้ที่ไฟล์ news_data.dart การเรียกใช้ NewsData ในไฟล์อื่น ต้องมีการเขียนคำสั่ง import 
import 'news_data.dart';

// สังเกตว่าประกาศเอาไว้ใน class _HomePageState แต่ไม่ได้เอาไว้ใน method build()
class _HomePageState extends State<HomePage> {
  //List<String> newsItems = ['abc', 'def', 'ghi'];

  // กำหนดชนิดของข้อมูล List เป็น NewsData
  // ทำให้ด้านใน List นี้ต้องเก็บข้อมูลแบบ Class NewsData เท่านั้น
  List<NewsData> newsItems = [
    // การสร้าง NewsData ต้องมีการกำหนดข้อความให้เป็นข้อมูล ของ content property ตามที่กำหนดไว้
    NewsData('abc'),
    NewsData('def'),
    NewsData('ghi')
  ];

  //...
```

## 2. ปรับให้ ListView ใช้ข้อมูลจาก newsItem

1. ลงมาที่ **ListView.separated()**
2. ใน itemBuilder เราปรับให้ข้อความใน ListTile ดึงค่าออกจาก newsItem โดยใช้เลข index

```dart
body: ListView.separated(
          itemCount: newsItems.length,
          itemBuilder: (BuildContext context, int index) {
            return ListTile(

              // คราวนี้ข้อมูลแต่ละตัวใน newsItem เป็นโครงสร้างแบบ newsData แล้ว ทำให้เราต้องเรียกใช้ข้อมูลผ่าน .content property 
              title: Text(newsItems[index].content),
            );
          },
          separatorBuilder: (context, index) {
            return Divider();
          },
        ));
```

บันทึกไฟล์ และทดสอบ

## ลองเล่น

ลองเพิ่ม NewsData เข้าไปใน newsItem List และสังเกตผล

```dart
 List<NewsData> newsItems = [
    NewsData('abc'),
    NewsData('def'),
    NewsData('ghi'),
    NewsData('สวัสดีวันอังคาร')
  ];
```

## ไฟล์ lib/home_page.dart ที่ปรับปรุงแล้ว 

```dart
// lib/home_page.dart
import 'package:flutter/material.dart';
import 'create_news_page.dart';
import 'news_data.dart';

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  //List<String> newsItems = ['abc', 'def', 'ghi'];

  List<NewsData> newsItems = [
    NewsData('abc'),
    NewsData('def'),
    NewsData('ghi')
  ];

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
              title: Text(newsItems[index].content),
            );
          },
          separatorBuilder: (context, index) {
            return Divider();
          },
        ));
  }
}

```