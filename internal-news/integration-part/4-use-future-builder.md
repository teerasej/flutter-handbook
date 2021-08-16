
# ใช้ FutureBuilder ควบคุมการแสดงผลให้สัมพันธ์กับการทำงานกับ Web API 

1. เปิดไฟล์ **lib/home_page.dart** 
2. เขียนคำสั่ง import http package มาใช้งาน 

```dart
import 'package:http/http.dart';
import 'package:http/http.dart' as http;
```

3. ใน class _HomePageState เราจะสร้างตัวแปรซึ่งเป็นตัวแทนของ URL ที่จะส่ง Request ไปที่ Web API 

```dart
class _HomePageState extends State<HomePage> {
  
  // ประเภทของข้อมูลนี้ เรียกว่า Uri
  // เราจะใช้ Url ของ ngrok เป็นเป้าหมายในการส่ง Request ของแอพเรา
  Uri webApi = Uri.parse('http://3242504417bb.ngrok.io/news');
```

4. ทำการ comment ส่วน body: เดิมก่อน เพื่อใช้อ้างอิง ListView ตอนหลัง
5. เขียนส่วน body: ใหม่​โดยใช้ FutureBuilder 

```dart
body: FutureBuilder(
    // เรียกใช้ http.get และกำหนด url เป้าหมายในการส่ง request 
    future: http.get(webApi),
    // builder function จะทำงานควบคู่กับ request ที่ส่งไปหา Web API
    // FutureBuilder จะส่งข้อมูลของ Request มาให้ builder function เป็นระยะ ในรูปแบบของตัวแปร snapshot 
    builder: (BuildContext context, AsyncSnapshot<Response> snapshot) {
        // ตอนนี้ builder function แสดงแค่ loading widget 
        return CircularProgressIndicator();
    },
)
```

6. บันทึกไฟล์​ และทดสอบผล


## ไฟล์ lib/home_page.dart ที่ปรับปรุงแล้ว 

```dart 
import 'package:flutter/material.dart';
import 'package:http/http.dart';
import 'package:news_app/create_news_page.dart';
import 'package:news_app/news_data.dart';

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