
# Build from Scratch 3 - สร้าง Widget ใช้เองแบบ Stateful

Flutter เตรียมระบบให้เราสามารถสร้าง Widget ของตัวเองขึ้นมาได้ ซึ่งมี 2 แบบ

1. Stateless
2. Stateful 

ในส่วนนี้เราจะใช้เป็น Stateful นะครับ

## 1. ไฟล์ lib/main.dart ต้องพร้อม

ไฟล์ `lib/main.dart` ต้องพร้อม แบบด้านล่างนี้

```dart
import 'package:flutter/material.dart';

void main() {
	runApp(MaterialApp(
        title: 'My Flutter',
        theme: ThemeData(
            primarySwatch: Colors.blue,
            brightness: Brightness.dark
        ),
        home: Scaffold(
            appBar: AppBar(
                title: Text('Counter'),
            ),
            body: Text('hello'),
        ),
    ));
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'My Flutter',
        theme: ThemeData(
            primarySwatch: Colors.blue,
            brightness: Brightness.dark
        ),
        home: Scaffold(
            appBar: AppBar(
                title: Text('Counter'),
            ),
            body: Text('hello'),
        ),
    );
  }
}
```

## 2. สร้าง Stateful Widget 

ถัดมาด้านล่างสุดของไฟล์ เราจะสร้าง Widget แบบ Stateless ขึ้นมา แล้วตั้งชื่อมันว่า `CounterArea`

```dart
import 'package:flutter/material.dart';

void main() {
	runApp(MaterialApp(
        title: 'My Flutter',
        theme: ThemeData(
            primarySwatch: Colors.blue,
            brightness: Brightness.dark
        ),
        home: Scaffold(
            appBar: AppBar(
                title: Text('Counter'),
            ),
            body: Text('hello'),
        ),
    ));
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'My Flutter',
        theme: ThemeData(
            primarySwatch: Colors.blue,
            brightness: Brightness.dark
        ),
        home: Scaffold(
            appBar: AppBar(
                title: Text('Counter'),
            ),
            body: Text('hello'),
        ),
    );
  }
}

// vvvvvvvvvvvv
// vvvvvvvvvvvv
// Stateful Widget มาในรูปแบบของ Class ในภาษาเชิง Object 
class CounterArea extends StatefulWidget {
  @override
  _CounterAreaState createState() => _CounterAreaState();
}

class _CounterAreaState extends State<CounterArea> {
  @override
  Widget build(BuildContext context) {
    return Container(
      
    );
  }
}
```

## 3. กำหนดหน้าตาของ Stateful Widget 

เราจะย้าย `Scaffold` widget มาเป็นหน้าตาของ `CounterArea` widget ผ่าน method `build()`

```dart
lass MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'My Flutter',
        theme: ThemeData(
            primarySwatch: Colors.blue,
            brightness: Brightness.dark
        ),
        // หลังจากย้าย Scaffold ไปไว้ใน CounterArea แล้ว ก็เอามาใช้แทน
        home: CounterArea()
    );
  }
}

class CounterArea extends StatefulWidget {
  @override
  _CounterAreaState createState() => _CounterAreaState();
}

class _CounterAreaState extends State<CounterArea> {
  @override
  Widget build(BuildContext context) {
    
    // ย้าย Scaffold widget มาจาก MyApp Widget
    return Scaffold(
            appBar: AppBar(
                title: Text('OK'),
            ),
            body: Text('hello'),
        );
  }
}
```

## 4. สร้างส่วน UI 

เขียน Widget เพิ่มในส่วนของ Scaffold

```dart
class CounterArea extends StatefulWidget {
  @override
  _CounterAreaState createState() => _CounterAreaState();
}

class _CounterAreaState extends State<CounterArea> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Counter'),
      ),
      // Center widget ไว้จัด widget อื่นๆ ให้อยู่ตรงกลาง
      body: Center(
          // Column widget ไว้จัด widget เรียงจากบนลงล่าง แบบแถวตอนเรียงเดี่ยว
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '0',
              // text widget กำหนด style ได้ด้วยนะ
              style: TextStyle(fontSize: 60),
            ),
          ],
        ),
      ),
      // FloatingActionButton เป็น widget กลมๆ ด้านล่างขวา
      floatingActionButton: FloatingActionButton(
        onPressed: () {},
        tooltip: 'Increment',
        child: Icon(Icons.add),
      ),
    );
  }
}
```

## 5. ประกาศตัวแปร ใช้ใน Widget 

เราสามารถประกาศตัวแปรใช้ใน Widget ได้ เพื่อประโยชน์ในการอัพเดตค่าต่างๆ 

```dart
class CounterArea extends StatefulWidget {
  @override
  _CounterAreaState createState() => _CounterAreaState();
}

class _CounterAreaState extends State<CounterArea> {

  // กำหนดตัวแปรเป็นจำนวนนับ
  int _counter = 0;


  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('OK'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              'You have pushed the button this many times:',
            ),
            Text(
              // ใช้เครื่องหมาย $ หรือ ${} ในการแทรกตัวแปรมาใช้ใน Widget ต่างๆ   
              '$_counter',
              style: TextStyle(fontSize: 60),
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {},
        tooltip: 'Increment',
        child: Icon(Icons.add),
      ),
    );
  }
}
```

## 6. ใช้คำสั่ง setState ในการอัพเดตค่า และ Widget

method `setState()` จะทำให้ Widget ทำการ build ตัวเองใหม่อีกครั้ง เราใช้ในการอัพเดตการเปลี่ยนแปลงให้เกิดใน User Interface

```dart
class CounterArea extends StatefulWidget {
  @override
  _CounterAreaState createState() => _CounterAreaState();
}

class _CounterAreaState extends State<CounterArea> {

  int _counter = 0;


  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('OK'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: TextStyle(fontSize: 60),
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          // การกำหนดค่าใหม่ให้ตัวแปรใน method setState() จะทำให้ Widget เรียกใช้ method build() ตัวเองอีกครั้ง
          setState(() {
            ++_counter;
          });
        },
        tooltip: 'Increment',
        child: Icon(Icons.add),
      ),
    );
  }
}
```
