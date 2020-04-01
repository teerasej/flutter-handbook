
# Navigation 

## 1. เตรียมไฟล์ 

สร้างโปรเจคใหม่ หรือใช้โปรเจคเดิมที่มีอยู่แล้ว 

เปิดไฟล์ `lib/main.dart` แล้วแทนที่โค้ดเดิม ด้วยโค้ดด้านล่าง

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {

  List numberList = ['a','b','c','d','e','f'];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Contact'),
      ),
      body: ListView.builder(
        itemCount: numberList.length, 
        itemBuilder: (BuildContext context, int index) {
          return ListTile(
            title: Text(numberList[index].toString()),
          );
        },
      ),
    );
  }
}
```

## 2. สร้างหน้าแอพ Detail Page 

สร้างไฟล์ `lib/detail_page.dart` และเขียนสร้าง Widget ชื่อ `DetailPage` ตามด้านล่าง

```dart

import 'package:flutter/material.dart';

class DetailPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('ชื่อ'),
      ),
      body: Container(

        padding: EdgeInsets.all(10.0),
        child: Column(
          children: <Widget>[
            // Image.network(contact.imageUrl, width: 200.0, height: 200.0,),
          ],
        ),
      ),
      bottomNavigationBar: Container(
        padding: EdgeInsets.fromLTRB(10, 0, 10, 30),
        child: RaisedButton(
              onPressed: () {

              },
              child: Row(
                mainAxisAlignment: MainAxisAlignment.center,
                children: <Widget>[
                  Icon(Icons.phone),
                  Text('Call')
                ],
              ),
            ),
      ),

    );
  }
}
```

## 3. ทำให้เกิดการเปิดไปหน้า Detail Page 

เปิดไฟล์ `lib/main.dart` 

เราจะเขียนคำสั่ง import โค้ดที่เราสร้างไว้ในไฟล์ `lib/detail_page.dart`

```dart
import 'detail_page.dart';
```

จากนั้นเราจะเพิ่ม `onTap:` property ให้กับ ListTile 

```dart
class _MyHomePageState extends State<MyHomePage> {

  List numberList = ['a','b','c','d','e','f'];


  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Contact'),
      ),
      body: ListView.builder(
        itemCount: numberList.length, 
        itemBuilder: (BuildContext context, int index) {
          return ListTile(
            title: Text(numberList[index].toString()),
            // เพิ่ม onTap property
            onTap: () {
                // ใช้ Navigator.push() ในการ render widget ชื่อ DetailPage ที่เราสร้าง และ import เข้ามาใช้
                Navigator.push(
                  context,
                  MaterialPageRoute(builder: (context) => DetailPage())
                );
            },
          );
        },
      ),
    );
  }
}
```

จากนั้นทดสอบรันแอพพลิเคชั่นดู