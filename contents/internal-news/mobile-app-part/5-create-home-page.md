
# สร้างหน้า Home Page 

ส่วนประกอบต่างๆ ของแอพ สามารถเรียกเป็น Widget ได้ ไม่ว่าจะเป็นหน้าแอพทั้งหน้า หรือแค่โลโก้รูปธรรมดา 

ในที่นี้เราจะสร้างหน้า Home Page สำหรับแสดงข่าวที่เก็บไว้ใน database

## 1. สร้างไฟล์ home_page.dart

- เราจะสร้างไฟล์ **home_page.dart** ไว้ใน **lib** directory 
- ให้คลิกขวา และเลือก **New file**

<img width="452" alt="2021-08-15_17-34-07" src="https://user-images.githubusercontent.com/85179/129475911-dbb6f78b-b95f-4f3b-96e3-90478df2304e.png">

- ตั้งชื่อว่า **home_page.dart**

## 2. สร้าง Stateful Widget

เรียกใช้ code snippet โดยพิมพ์ **st** อีกครั้งแต่คราวนี้เลือกเป็น **Stateful Widget**
และให้ตั้งชื่อว่า HomePage 

<img width="741" alt="2021-08-15_17-59-38" src="https://user-images.githubusercontent.com/85179/129476191-737c8ea8-91f3-4daf-8ada-c550b26f1427.png">


- จะสังเกตว่าโค้ดประกาศ Stateful Widget มีขนาดใหญ่กว่า Stateless Widget
- สังเกตว่าชื่อไฟล์จะตั้งชื่อเป็นตัวเล็กทั้งหมด (_home_page.dart_) แต่ชื่อ Widget จะตั้งแบบเขียนติดกันและขึ้นต้นด้วยตัวใหญ่ในแต่ละคำ (_HomePage_) คล้ายหลังอูฐ (เรียกเทคนิคนี้ว่า Camel case)

## 3. import package:flutter/material.dart

อย่าลืมว่า flutter เป็นชุดโค้ดคำสั่งที่มีการเตรียมไว้ให้เราเรียกใช้ในโปรเจค สำหรับสร้างหน้าตาของแอพพลิเคชั่นของเรา

การเรียกใช้ Widget หรือคำสั่งต่างๆ ต้องมีการอ้างถึงที่อยู่ของโค้ดที่เรียกด้วย เช่นการอ้างถึง Widget ต่างๆ หรือแม้แต่ function 

จะสังเกตว่าถึงแม้เราจะใช้ snippet วางโค้ดมา แต่โค้ดบางจุดจะมีการขีดเส้นใต้สีแดงไว้ ส่วนนี้คือ error ในการเขียนโค้ด ซึ่ง Visual studio code จะตรวจจับได้ก่อนที่จะรันแอพให้ทำงาน 

<img width="771" alt="2021-08-15_18-01-57" src="https://user-images.githubusercontent.com/85179/129476256-440c3807-defe-4827-ba56-2d54d0855b70.png">


1. ให้เราทำการคลิกเลือก โค้ดที่ถูกขีดเส้นใต้สีแดงนี้ 
2. จะปรากฎไอคอนรูปหลอดไฟ บริเวณเลขบรรทัดของโค้ด ให้คลิก จะมีเมนูแสดงขึ้นมา
3. เลือกคำสั่ง **import library package:flutter/material.dart**

<img width="702" alt="2021-08-15_18-03-01" src="https://user-images.githubusercontent.com/85179/129476367-3b6a6fcc-60e2-4c55-90e9-5ea53b6dcb8d.png">

จะได้โค้ดดังด้านล่าง อย่าลืมกำหนดแก้ไขให้ Container widget ใน method `build()` ให้เป็นเหมือนในตัวอย่างด้านล่าง

```dart
// lib/home_page.dart

import 'package:flutter/material.dart';

class HomePage extends StatefulWidget {
  HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {

    // แก้ไขโค้ดที่ติดมาให้เป็นแบบด้านล่าง เพราะค่า null ใน child property จะทำให้เกิด error
    return Container(
        child: Text('Home Page')
    );
  }
}
```

## ไฟล์ lib/home_page.dart ที่ปรับปรุงแล้ว 

```dart
// lib/home_page.dart

import 'package:flutter/material.dart';

class HomePage extends StatefulWidget {
  HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {
    return Container(
        child: Text('Home Page')
    );
  }
}
```
