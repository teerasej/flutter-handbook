
# รีเฟรชหน้าแอพหลังจากเปิดกลับมาที่หน้า Home 

1. เปิดไฟล์ **lib/home_page.dart** 
2. เรียกใช้ function `Navigator.push().then()` เพื่อรันคำสั่ง setState ที่ทำให้ Widget `build` ตัวเองใหม่ 

```dart
floatingActionButton: FloatingActionButton(
    child: Icon(Icons.add),
    onPressed: () {
    print('click');

    Navigator.push(
        context,
        MaterialPageRoute(builder: (context) {
        return CreateNewsPage();
        }),
    // เรียกใช้ function Navigator.push().then()
    // เพื่อรันคำสั่ง setState ที่ทำให้ Widget build ตัวเองใหม่ 
    ).then((value) {
        setState(() {});
    });
    },
),
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

  Uri webApi = Uri.parse('http://c8d8b4dc6a7d.ngrok.io/news');

  // List<NewsData> newsItems = [
  //   NewsData(id: '1', content: 'abc'),
  //   NewsData(id: '2', content: 'def'),
  //   NewsData(id: '3', content: 'ghi')
  // ];

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

            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) {
                return CreateNewsPage();
              }),
            ).then((value) {
              setState(() {});
            });
          },
        ),
        body: FutureBuilder(
          future: http.get(webApi),
          builder: (BuildContext context, AsyncSnapshot<Response> snapshot) {
            if (snapshot.hasError) {
              return Text('opps... ${snapshot.error.toString()}');
            }

            if (snapshot.connectionState == ConnectionState.done) {
              var jsonFromAPI = snapshot.data?.body as String;
              var newsItems = newsDataFromJson(jsonFromAPI);

              return ListView.separated(
                itemCount: newsItems.length,
                itemBuilder: (BuildContext context, int index) {
                  return ListTile(
                    title: Text(newsItems[index].content),
                  );
                },
                separatorBuilder: (context, index) {
                  return Divider();
                },
              );
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