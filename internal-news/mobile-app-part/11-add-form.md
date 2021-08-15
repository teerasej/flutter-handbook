
# เพิ่มแบบฟอร์มสำหรับกรอกเนื้อหาข่าวใหม่

เปิดไฟล์ **lib/create_news_page.dart**


## 1. สร้างหน้าตาโดยใช้ Widget กำหนด

```dart
import 'package:flutter/material.dart';

class CreateNewsPage extends StatefulWidget {
  const CreateNewsPage({Key? key}) : super(key: key);

  @override
  _CreateNewsPageState createState() => _CreateNewsPageState();
}

class _CreateNewsPageState extends State<CreateNewsPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Create News'),
      ),
      // เพิ่ม Widget ลงไปในส่วน Body เพื่อเป็นหน้าตาของแอพในหน้า Create News Page
      // ใช้ Padding เพื่อเว้นระยะห่างจากขอบจอ
      body: Padding(
        // กำหนดระยะห่างเป็น 10
        padding: EdgeInsets.all(10),

        // กำหนดด้านในของ Padding เป็น Form widget
        child: Form(

          // กำหนดด้านในของ Form widget เป็น column widget ซึ่ง widget อื่นๆด้านในนี้จะเรียงจากบนลงล่าง
          child: Column(

            // กำหนดให้ widget ด้านใน Column เรียงชิดซ้าย
            crossAxisAlignment: CrossAxisAlignment.start,

            // กำหนด Widget ที่อยู่ด้านใน column
            children: <Widget>[

             // กำหนดข้อความ โดยใช้ Text widget
              Text('เนื้อหาใหม่'),
             // กำหนดช่องกรอกข้อมูล โดยใช้ TextFormField widget
              TextFormField(),

             // กำหนดปุ่ม โดยใช้ ElevatedButton widget
              ElevatedButton(
                // กำหนด function ที่จะทำงานตอนปุ่มถูกกด
                onPressed: () {
                  print('do something');
                },
                // กำหนดด้านในของปุ่ม เป็นข้อความ ด้วย Text widget
                child: Text('เพิ่ม'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

```

บันทึกไฟล์ และทดสอบใช้งาน

## 2. เว้นระยะห่าง และควบคุมขนาดปุ่ม โดยใช้ SizedBox

ลงมาในส่วน **body:** ของ Scaffold เราจะเพิ่ม SizedBox ลงไปใน Column เพื่อสร้างระยะห่าง และกำหนดขนาดปุ่ม

การครอบ Widget สามารถทำได้ง่ายๆ โดยการ

1. กดเลือก Widget ที่ต้องการครอบ
2. คลิกไอคอนหลอดไฟ (Show fixes) ทางด้านซ้ายของบรรทัดโค้ดนั้น
3. เลือกคำสั่ง **Wrap with...** 

<img width="576" alt="2021-08-15_21-06-51" src="https://user-images.githubusercontent.com/85179/129481229-e5e7765a-8926-4c73-b547-636ccaab5795.png">


```dart
body: Padding(
        padding: EdgeInsets.all(10),
        child: Form(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: <Widget>[
              Text('เนื้อหาใหม่'),
              TextFormField(),
              // กำหนดความสูงของระยะห่าง ระหว่างช่องกรอกข้อมูล กับปุ่ม
              SizedBox(
                height: 20,
              ),
              // กำหนดความกว้างของปุ่ม โดยการครอบ ElevatedButton ด้วย SizedBox 
              SizedBox(
                width: double.infinity,
                child: ElevatedButton(
                  onPressed: () {
                    print('do something');
                  },
                  child: Text('เพิ่ม'),
                ),
              ),
            ],
          ),
        ),
      ),
```

บันทึกไฟล์ และทดสอบใช้งาน
