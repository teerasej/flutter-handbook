
# เช็คการทำงานของ snapshot ถ้าเสร็จสมบูรณ์ให้แสดงข้อความ

1. เปิดไฟล์ **lib/home_page.dart** 
2. ใน function builder เราจะเขียนส่วนที่เช็คค่าการทำงานของ Snapshot เพื่อแสดง Widget อื่นเมื่อ snapshot ยืนยันว่าการส่ง request เสร็จสมบูรณ์แล้ว 

```dart
body: FutureBuilder(
          future: http.get(webApi),
          builder: (BuildContext context, AsyncSnapshot<Response> snapshot) {

            // เช็ค .connectionState ของ Snapshot ว่าค่าเป็น done หรือยัง 
            if (snapshot.connectionState == ConnectionState.done) {
                // ถ้าใช่ ให้คืนค่า Text Widget จาก Builder function แทน
                return Text('Got data');
            }

            return CircularProgressIndicator();
          },
        )
```

## ไฟล์ lib/home_page.dart ที่ปรับปรุงแล้ว 

```dart 
import 'package:flutter/material.dart';
import 'package:http/http.dart';
import 'create_news_page.dart';
import 'news_data.dart';

import 'package:http/http.dart' as http;

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  //List<String> newsItems = ['abc', 'def', 'ghi'];

  Uri webApi = Uri.parse('http://3242504417bb.ngrok.io/news');

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
        body: FutureBuilder(
          future: http.get(webApi),
          builder: (BuildContext context, AsyncSnapshot<Response> snapshot) {
            if (snapshot.connectionState == ConnectionState.done) {
              return Text('Got data');
            }

            return CircularProgressIndicator();
          },
        )
        // body: ListView.separated(
        //   itemCount: newsItems.length,
        //   itemBuilder: (BuildContext context, int index) {
        //     return ListTile(
        //       title: Text(newsItems[index].content),
        //     );
        //   },
        //   separatorBuilder: (context, index) {
        //     return Divider();
        //   },
        // )
        );
  }
}

```