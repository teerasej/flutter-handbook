
# สร้าง Widget MyApp

- เราสามารถประกาศสร้าง Widget ขึ้นมาใช้งานเองในแอพได้
- Widget ของเราสามารถกำหนดชื่อ รวมถึงหน้าตา และการทำงานได้ 
- การประกาศ Widget ด้วยตัวเองนี้ จะใช้ลักษณะการเขียนโปรแกรมเชิง OOP โดยใช้ keyword `class` เป็นตัวกำหนด

## 1. ประกาศสร้าง Widget แบบ Stateless ชื่อ MyApp

ด้านล่าง ถัดจาก function `main()` เราจะทำการประกาศ Widget Class ของเราเอง

```dart
// lib/main.dart

import 'package:flutter/material.dart';

void main() {
  runApp(
    MaterialApp(
      title: 'News App',
      home: Text('oh, hello'),
    ),
  );
}

// --> เราจะประกาศสร้างไว้ด้านล่างนี้ นั่นคือ ไม่ได้อยู่ในขอบเขตของ function main()

```

- พิมพ์ **st** จะปรากฎรายชื่อของ Snippet ขึ้นมา 
- **Code snippet** เป็นเครื่องมือที่อำนวยความสะดวกในนักพัฒนาในการเขียนโปรแกรม โดยจะเป็นรายการของโค้ดโปรแกรมที่มีการพิมพ์เรียกใช้งานบ่อยๆ ทำให้ไม่ต้องพิมพ์เองทั้งหมด


## ไฟล์ lib/main.dart ที่ปรับปรุงเสร็จแล้ว

```dart
// lib/main.dart

import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'News App',
      home: Text('oh, hello'),
    );
  }
}


```