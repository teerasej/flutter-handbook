
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

1. พิมพ์ **st** จะปรากฎรายชื่อของ Snippet ขึ้นมา ให้เลือก **StatefulW** (Stateful Widget)
  - **Code snippet (โค้ดตัดแปะ)** เป็นเครื่องมือที่อำนวยความสะดวกในนักพัฒนาในการเขียนโปรแกรม โดยจะเป็นรายการของโค้ดโปรแกรมที่มีการพิมพ์เรียกใช้งานบ่อยๆ ทำให้ไม่ต้องพิมพ์เองทั้งหมด

<img width="628" alt="2021-08-15_17-00-28" src="https://user-images.githubusercontent.com/85179/129475129-b9affb09-28d3-4287-8878-64c3d286ad0b.png">

2. จะได้โค้ดตัดแปะตามด้านล่าง ให้สังเกตว่ามีการไฮท์ไลท์ที่คำว่า name 2 จุด 

<img width="662" alt="2021-08-15_17-20-00" src="https://user-images.githubusercontent.com/85179/129475235-17fdfe95-a235-45d8-b9ce-58513ca152e0.png">


3. ให้พิมพ์ตั้งชื่อว่า MyApp จะเห็นว่าชื่อที่พิมพ์จะแทนที่ไฮท์ไลท์ 2 จุดดังกล่าว

<img width="657" alt="2021-08-15_17-22-03" src="https://user-images.githubusercontent.com/85179/129475251-582a8258-617a-4893-8fe8-6038556e1ba5.png">

4. ลบ Container widget ที่มากับ Code snippet ทิ้ง จะเหลือแค่คำสั่ง return 

<img width="671" alt="2021-08-15_17-22-49" src="https://user-images.githubusercontent.com/85179/129475480-e553072b-ab7e-41a2-b421-faad8aa99e9d.png">


5. เราจะย้าย MaterialApp Widget ที่ประกาศไว้ใน function `runApp()` มาเป็นส่วนที่จะถูก return ออกจาก MyApp widget

<img width="700" alt="2021-08-15_17-26-48" src="https://user-images.githubusercontent.com/85179/129475485-4af07799-83d9-4571-bac8-478335ca4bad.png">


6. จากนั้นภายใน function runApp() เราจะประกาศเรียกใช้ MyApp widget แทน ซึ่ง MyApp widget จะถูกเรียกใช้ build() เพื่อให้ในการแสดงหน้าตาขึ้นมาบนแอพ (ในที่นี้ก็คือ MaterialApp ตัวเดิม)

```dart
// lib/main.dart

//...

void main() {
  runApp(MyApp());
}

//...
```

7. จะได้โค้ดแบบด้านล่าง 

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

8. บันทึกไฟล์ และทดสอบจะสังเกตว่าผลที่ได้ไม่เปลี่ยนแปลง แต่ในโค้ดของเรา มีการประกาศสร้าง Widget MyApp ของเราเองแล้ว และเราสามารถใช้เทคนิคเดียวกันนี้ในการแบ่งส่วนหน้าตาของแอพออกเป็นหน้าต่างๆ ได้


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
