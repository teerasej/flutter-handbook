
# ListView widget

## 1. เตรียมโปรเจค

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
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Contact'),
      ),
      body: Container(),
    );
  }
}
```

## 2. สร้าง ListView widget แบบตายตัว

`ListView` เป็น Widget ตัวหนึ่ง ที่สามารถแสดงเป็นรายการในแอพได้ 

เราสามารถกำหนด Widget อื่นๆ ลงไปใน `ListView` widget ได้ แต่ทั่วไปก็คือ `ListTile` widget

```dart
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Contact'),
      ),
      // ใช้งาน ListView widget แทน
      body: ListView(
        children: <Widget>[
          // เราสามารถกำหนด ListTile widget ใช้งานใน Listview widget ได้
          ListTile(
            leading: Text('1'),
            title: Text('Iron Mann'),
          ),
          ListTile(
            leading: Text('2'),
            title: Text('Captain America'),
          ),
          ListTile(
            leading: Text('3'),
            title: Text('Thor'),
          )
        ],
      ),
    );
  }
}
```

## 3. สร้าง ListView widget แบบ Dynamic 

แต่ในความจริงแล้ว เรามักจะใช้ ListView widget แบบที่แสดงจำนวนแถวตามข้อมูลมากกว่า 

โดยเราสามารถใช้ method ของ ListView ที่ชื่อ `ListView.builder()` ดังนี้ 

- `itemCount`: จำนวนแถวที่ ListView ต้องสร้าง
- `itemBuilder`: method ที่คืนค่าเป็น widget ให้ ListView เอาไปใช้งาน 

```dart
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Contact'),
      ),
      // ใช้ Method ListView.builder() ในการสร้าง ListView จากข้อมูลตัวแปรแทน
      body: ListView.builder(
        // ฟังก์ชั่นที่ต้อง return ค่าเป็น Widget เอาไปสร้าง ListView
        itemBuilder: (BuildContext context, int index) {
          return ListTile(
            title: Text('ok'),
          );
        },
        // จำนวน Widget ที่ต้องสร้าง
        itemCount: 10, 
      ),
    );
  }
```

ซึ่งเราสามารถใช้ `List` (Array ของภาษา Dart) มาเป็นตัวอ้างอิงค่า property ของ method `ListView.builder()` ได้เช่นเดียวกัน

```dart
class _MyHomePageState extends State<MyHomePage> {

  // สร้าง List ขึ้นมา
  List numberList = ['a','b','c','d','e','f'];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Contact'),
      ),
      body: ListView.builder(
        itemBuilder: (BuildContext context, int index) {
          return ListTile(
            // ใช้เลข index ที่ส่งเข้ามาใน function ในการดึงข้อมูลออกมาให้ตรงกับที่แสดงใน ListView
            title: Text(numberList[index].toString()),
          );
        },
        // จำนวนอ้างอิงจากข้อมูลที่อยู่ใน numberList 
        itemCount: numberList.length, 
      ),
    );
  }
}
```