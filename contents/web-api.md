
# Web API

- [Start Project](https://www.dropbox.com/s/52iznb5rzep7ubj/nextflow_api_start.zip?dl=0)
- [Finish Project](https://www.dropbox.com/s/k7j790b68fv48bn/nextflow_api_finish.zip?dl=0)

## 1. เพิ่ม package

เราจะมีการใช้ package เพิ่มเติม ให้เปิดไฟล์ `pubspec.yaml` และเพิ่มชื่อ [http package](https://pub.dev/packages/http) ในส่วนของ dependecies ดังนี้

```yaml
dependencies:
  http: ^0.12.0+4
```

บันทึกไฟล์ ถ้าใช้ Visual Studio Code จะมีการดาวน์โหลด package มาติดตั้งให้เองอัตโนมัติ

### ถ้าต้องการบังคับให้ Flutter ดาวน์โหลด package ด้วยตัวเอง

รันคำสั่งใน terminal ดังนี้

```bash
flutter package get
```

## 2. สร้าง Class Dart จาก JSON

อย่างแรกให้เราสร้างไฟล์ `lib/contact_model.dart` ขึ้นมา

จากนั้น เปิดเว็บ [randomuser.me](https://randomuser.me/documentation#howto) เพื่อดูตัวอย่างข้อมูลที่เราจะได้จาก WebAPI

และ URL ที่เราจะใช้ใน lab นี้คือด้านล่าง

```
https://randomuser.me/api/?results=50
```

```
// หรืออยากลอง
https://nextflow.in.th/api/water-meter/data.json
```

และเราจะทำการ copy JSON ที่ได้จาก URL ดังกล่าวมาใช้ใน ตัว Class Generator เพื่อสร้างไฟล์ dart ที่เป็นตัวแทนของ class ต่างๆ 

- [JSON to dart](https://javiercbk.github.io/json\_to\_dart/)
- [QuickType.io](https://quicktype.io/)

เสร็จแล้วให้ copy code ที่ได้ทั้งหมดมาใส่ไว้ใน `contact_model.dart`

- [ลิ้งค์ตัวอย่างโค้ดที่ได้จาก QuickType.io](https://gist.github.com/teerasej/f36fd04fe0dc5a1a846b4352cf82db6e)

## 3. ใช้งาน class ใน Widget 

เปิดไฟล์ `lib/main.dart`

```dart
import 'contact_model.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';
```

จากนั้นเราจะเริ่มส่ง request ไปที่ Web API ดังกล่าวใน method ชื่อ initState ซึ่ง widget จะเรียกใช้ method 1 ครั้งตอนโหลดแสดงหน้าตาของ Widget 

```dart
class _MyHomePageState extends State<MyHomePage> {

  List contactList = [
    ContactModel(name: 'Nextflow', phone: '083-071-3373'),
    ContactModel(name: 'Peter Parker', phone: '089-322-1223'),
    ContactModel(name: 'Jazz', phone: '084-212-3290'),
  ];

  // เรียกใช้งาน method getData() ตอนแสดง MyHomePage
  @override
  void initState() {
    super.initState();
    getData();
  }

  // สร้างตัวแปร contactResult จากที่ได้จากเว็บ quicktype.io
  RandomUserResult contactResult = new RandomUserResult();
  // สร้าง method getData() เพื่อเรียกข้อมูลจาก Web API
  void getData() async {

    // ส่ง request เพื่อขอข้อมูลจาก Web API 
    final response = await http.get('https://randomuser.me/api/?results=50');
    // แสดงข้อความ JSON 
    print(response.body);
    
    // แปลงข้อมูล JSON ให้ใช้งานในรูปแบบของ Object ใน
    var randomUserResult =
        RandomUserResult.fromJson(json.decode(response.body));
    
    // กำหนด state เพื่อให้ widget สร้างตัวเองใหม่
    setState(() {
      contactResult = randomUserResult;
    });
  }

  
```

## 4. แก้ไขให้ ListView 

ลงมาในส่วน method `build()` เราจะแก้ให้ ListView ดึงข้อมูลจาก contactResult แทน 

```dart
@override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Contact'),
      ),
      body: ListView.builder(
        itemCount: contactResult.results.length,
        itemBuilder: (BuildContext context, int index) {

          // ใช้เลข index ดึงข้อมูลจาก contactResult ตามลำดับข้อมูล JSON
          var contact = contactResult.results[index];

          return ListTile(
            // ใช้ Image widget แสดงรูปจากอินเตอร์เน็ต
            leading: Image.network(
              contact.picture.large,
              width: 50.0,
              height: 50.0,
              fit: BoxFit.scaleDown,
            ),
            // แสดงชื่อ นามสกุล
            title: Text(
              "${contact.name.first} ${contact.name.last}",
              style: TextStyle(fontWeight: FontWeight.w700),
            ),
            // แสดงเบอร์โทรศัพท์
            subtitle: Text(
              contact.phone,
              style: TextStyle(color: Colors.grey),
            ),
            onTap: () {
              Navigator.push(context, MaterialPageRoute(builder: (context) {
                return DetailPage(contact);
              }));
            },
          );
        },
      ),
    );
  }
```

## 5. ส่งข้อมูลไปยังหน้า Detail Page

เปิดไฟล์ `lib/detail_page.dart` เราจะแก้ไขให้ Widget ตัวนี้สามารถรับค่า Result จาก `main.dart` ได้ 

```dart

// import โค้ดที่เราสร้างไว้ในไฟล์ contact_model.dart
import 'contact_model.dart';
import 'package:flutter/material.dart';

class DetailPage extends StatelessWidget {

  // สร้างตัวแปรเก็บค่า Result ที่จะส่งมาจากหน้าแรก
  Result contact;

  // กำหนดให้ DetailPage รับค่ามาเก็บไว้ได้
  DetailPage(this.contact);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        // แสดงชื่อ และนามสกุลจากตัวแปร contact
        title: Text("${contact.name.first} ${contact.name.last}"),
      ),
      body: Container(

        padding: EdgeInsets.all(10.0),
        child: Column(
          children: <Widget>[
            // แสดงรูปจากอินเตอร์เน็ต ตามข้อมูลในตัวแปร contact
            Image.network(contact.picture.large, width: 200.0, height: 200.0,),
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

## 6. ส่งข้อมูลที่เลือกจาก MyHomePage ไปให้ DetailPage

ในส่วนของ `onTap:` property เราจะแก้ไขให้ส่งตัวแปร contact ไปให้หน้า Detail 

```dart
onTap: () {
    Navigator.push(context,
        MaterialPageRoute(builder: (context) {
        return DetailPage(contact);
        }
      ));
},
```

## ไฟล์สมบูรณ์

- [โค้ดในไฟล์ main.dart](https://gist.github.com/teerasej/6c4f0d8eb763b201efdc7b1f62b88121)
- [โค้ดในไฟล์ detail_page.dart](https://gist.github.com/teerasej/330994f7a7bfe82fb1868bed3667f624)
