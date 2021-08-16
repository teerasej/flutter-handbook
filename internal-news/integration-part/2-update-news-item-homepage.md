
# แก้ไขให้ newsItems ใช้รูปแบบ class ใหม่

เนื่องจากโครงสร้างของ Class NewsItem เปลี่ยนไปจากการที่เราเอาโค้ดของ QuickType มาใช้แทนที่เราเขียนเอง ทำเกิด Error ในหน้า home page ที่มีการเรียกใช้ NewsItem 

1. เปิดไฟล์​ **lib/home_page.dart**
2. แก้ไขการกำหนดข้อมูลจำลองของ Class NewsItem 

```dart 
class _HomePageState extends State<HomePage> {
  //List<String> newsItems = ['abc', 'def', 'ghi'];

  List<NewsData> newsItems = [
    // เนื่องจาก constructor ของ NewsData ที่ได้จาก QuickType กำหนดให้ต้องมีค่า id ด้วย ในตัวอย่าง List ของเราเลยต้องทำตามเงื่อนไข
    NewsData(id: '1', content: 'abc'),
    NewsData(id: '2', content: 'def'),
    NewsData(id: '3', content: 'ghi')
  ];
```


## ไฟล์ lib/home_page.dart ที่ปรับปรุงแล้ว 

```dart
import 'package:flutter/material.dart';
import 'package:news_app/create_news_page.dart';
import 'package:news_app/news_data.dart';

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  //List<String> newsItems = ['abc', 'def', 'ghi'];

  List<NewsData> newsItems = [
    NewsData(id: '1', content: 'abc'),
    NewsData(id: '2', content: 'def'),
    NewsData(id: '3', content: 'ghi')
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