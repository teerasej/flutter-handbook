
# ใช้ Material App Widget เป็นตัวแทนของแอพ

## 1. สร้างหน้าตาของแอพ

เราจะเรียกใช้ function `runApp()` เพื่อเริ่มสร้างหน้าตาของแอพ 

```dart
// lib/main.dart

import 'package:flutter/material.dart';

void main() {
  // print('hello');
  runApp();
}
```

## 2. ด้วยการประกาศสิ่งที่เรียกว่า widget

ในที่นี้เราเริ่มประกาศตัวแอพขึ้นมา ด้วย widget ที่ชื่อ **MaterialApp** 


```dart
// lib/main.dart

import 'package:flutter/material.dart';

void main() {
  // print('hello');
  runApp(
    // ประกาศใช้ MaterialApp widget
    // กำหนดชื่อเรียกว่า 'News App' ผ่าน property ชื่อ title
    MaterialApp(
      title: 'News App',
      // ประกาศใช้ Text Widget โดยกำหนดข้อความลงไปให้กับ widget
      // Text widget ถูกกำหนดใช้เป็น property ชื่อ home ของ MaterialApp
      home: Text('oh, hello'),
    ),
  );
}
```

## 3. ทดสอบแอพ

บันทึกไฟล์ และทดสอบผล
เราน่าจะเห็นข้อความ **oh, hello** ตามที่กำหนดไว้ใน **Text** Widget



## ไฟล์ lib/main.dart ที่ปรับปรุงเสร็จแล้ว

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    MaterialApp(
      title: 'News App',
      home: Text('oh, hello'),
    ),
  );
}

```