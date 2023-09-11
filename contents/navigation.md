
# Navigation 

- [Start Project](https://www.dropbox.com/s/wf0yszojv2ge24f/nextflow_nav_start.zip?dl=0)
- [Finish Project](https://www.dropbox.com/s/61npw89ay9t5ywc/nextflow_nav_finish.zip?dl=0)

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
      title: 'Nextflow Contact',
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
  

  List contactList = [
    ContactModel(name: 'Nextflow', phone: '083-071-3373'),
    ContactModel(name: 'Peter Parker', phone: '089-322-1223'),
    ContactModel(name: 'Jazz', phone: '084-212-3290'),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Contact'),
      ),
      body: ListView.builder(
        itemBuilder: (BuildContext context, int index) {

          var contact = contactList[index] as ContactModel;

          return ListTile(
            title: Text(contact.name),
            subtitle: Text(contact.phone)
          );
        },
        itemCount: contactList.length, 
      ),
    );
  }
}


class ContactModel { 

  String name;
  String phone;

  ContactModel({this.name = "Nextflow", this.phone = "083-071-3373"});
}
```

## 2. สร้างหน้าแอพ Detail Page 

สร้างไฟล์ `lib/detail_page.dart` และเขียนสร้าง Widget ชื่อ `DetailPage` ตามด้านล่าง

```dart
import 'package:flutter/material.dart';
import 'main.dart';

class DetailPage extends StatelessWidget {

  ContactModel contact;

  DetailPage(this.contact);


  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('${contact.name}'),
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
                  Text('Call ${contact.phone}')
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
  

  List contactList = [
    ContactModel(name: 'Nextflow', phone: '083-071-3373'),
    ContactModel(name: 'Peter Parker', phone: '089-322-1223'),
    ContactModel(name: 'Jazz', phone: '084-212-3290'),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Contact'),
      ),
      body: ListView.builder(
        itemBuilder: (BuildContext context, int index) {

          var contact = contactList[index] as ContactModel;

          return ListTile(
            title: Text(contact.name),
            subtitle: Text(contact.phone),
            // เพิ่ม onTap property
            onTap: () {
                // ใช้ Navigator.push() ในการ render widget ชื่อ DetailPage ที่เราสร้าง และ import เข้ามาใช้
                Navigator.push(context, MaterialPageRoute(builder: (context) {
                return DetailPage(contact);
              }));
            },
          );
        },
      ),
    );
  }
}
```

จากนั้นทดสอบรันแอพพลิเคชั่นดู

## ไฟล์เต็ม 

### lib/main.dart 

```dart
import 'package:flutter/material.dart';
import 'package:nextflow_nav/detail_page.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Nextflow Contact',
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
  

  List contactList = [
    ContactModel(name: 'Nextflow', phone: '083-071-3373'),
    ContactModel(name: 'Peter Parker', phone: '089-322-1223'),
    ContactModel(name: 'Jazz', phone: '084-212-3290'),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Contact'),
      ),
      body: ListView.builder(
        itemBuilder: (BuildContext context, int index) {

          var contact = contactList[index] as ContactModel;

          return ListTile(
            title: Text(contact.name),
            subtitle: Text(contact.phone),
            onTap: () {
              Navigator.push(context, MaterialPageRoute(builder: (context) {
                return DetailPage(contact);
              }));
            },
          );
        },
        itemCount: contactList.length, 
      ),
    );
  }
}


class ContactModel { 

  String name;
  String phone;

  ContactModel({this.name = "Nextflow", this.phone = "083-071-3373"});
}
```
