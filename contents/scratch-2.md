
# Build from Scratch 2 - สร้าง Widget ใช้เองแบบ Stateless

Flutter เตรียมระบบให้เราสามารถสร้าง Widget ของตัวเองขึ้นมาได้ ซึ่งมี 2 แบบ

1. Stateless
2. Stateful 

ในส่วนนี้เราจะใช้เป็น Stateless นะครับ

## 1. ก่อนเริ่มโปรเจค 

โค้ดในไฟล์ `lib/main.dart` ต้องพร้อมแบบด้านล่าง

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    MaterialApp(
      theme: ThemeData(
        primarySwatch: Colors.purple,
        brightness: Brightness.dark,
      ),
      title: 'My Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Counter'),
        ),
        body: Text('hello'),
      ),
    ),
  );
}
```

## 2. สร้าง Stateless Widget 

ถัดมาด้านล่างสุดของไฟล์ เราจะสร้าง Widget แบบ Stateless ขึ้นมา แล้วตั้งชื่อมันว่า `MyApp`

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
// vvvvvvvvvvvv
// vvvvvvvvvvvv
// Stateless Widget มาในรูปแบบของ Class ในภาษาเชิง Object 
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      
    );
  }
}
```

## 3. กำหนดหน้าตาของ Widget ผ่าน method `build`

Widget ทุกตัวจะมี method `build()` ที่จะคืนค่าออกไปเป็น Widget 

ดังนั้นเราลองให้มัน return ออกไปเป็น `Text()` widget ง่ายๆ สักตัว

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Text('MyApp นะจ๊ะ');
  }
}
```

และลองเอาไปใช้ ใน `Scaffold()` widget ดู

```dart
home: Scaffold(
    appBar: AppBar(
        title: Text('Counter'),
    ),
    body: MyApp(),
),
```

ทดสอบรันแอพของเรา จะเห็นว่ามีข้อความแสดงขึ้นมา ซึ่งเป็นตัวตนของ MyApp widget ที่เราสร้าง และเอามาใช้

**จบขั้นตอนนี้แล้ว อย่าลืมลบ `MyApp()` Widget ที่ใช้ใน `home:` ออกไป แล้วใส่ `Container()` คืนด้วยนะ**

## 4. กำหนดหน้าตาของ MyApp เป็น Scaffold แทน

เป้าหมายที่แท้จริงของเรา คือการแยก `MaterialApp()` widget ออกมาจาก method `runApp()` นั่นเอง 

ดังนั้นให้เราย้าย `Scaffold()` widget ออกมาใส่ไว้ใน `MyApp()` Widget แทน

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'My Flutter',
        theme: ThemeData(
            primarySwatch: Colors.purple,
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

จากนั้นเอา MyApp widget มาใช้ใน method `runApp()` แทน

```dart
import 'package:flutter/material.dart';

void main() {
  // เอา MyApp widget มากำหนดเป็นใช้แทน MaterialApp Widget
	runApp(MyApp());
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

## 5. บันทึกไฟล์ และทดสอบรันใช้งาน