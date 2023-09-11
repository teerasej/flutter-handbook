

# ดึงข้อมูลออกมาจากแบบฟอร์ม เตรียมส่งให้ Web API

## 1. สร้างตัวเชื่อมโยงกับ TextFormField

1. เปิดไฟล์ **lib/create_news_page.dart** 
2. ลงมาที่ **class _CreateNewsPageState**
3. เราจะสร้างตัวแปร **TextEditingController**
4. ตั้งชื่อว่า **newsContentController** เพื่อใช้กับ **TextFormField** Widget 

```dart
class _CreateNewsPageState extends State<CreateNewsPage> {
  
  // ตัวแปรประเภท TextEditingController มักใช้ในการเชื่อมโยงกับ TextFormField Widget 
  TextEditingController newsContentController = TextEditingController();

```

## 2. กำหนด TextEditingController ให้กับ TextFormField

ลงมาที่ **TextFormField** widget ให้กำหนด `controller:` property ด้วย **newsContentController** ที่สร้างขึ้นมา

```dart
TextFormField(
    controller: newsContentController,
),
```

## 3. เข้าถึงค่าใน TextFormField ผ่าน 

```dart
ElevatedButton(
    onPressed: () {
        // TextEditingController ที่กำหนดให้กับ Widget แล้ว จะสามารถควบคุมตัว Widget รวมถึงดึงข้อมูลออกมาได้ด้วยผ่าน .text
        var newsContent = newsContentController.text;
        print(newsContent);
    },
    child: Text('เพิ่ม'),
),
```

## ไฟล์ lib/create_news_page.dart ที่ปรับปรุงแล้ว 

```dart
import 'package:flutter/material.dart';

class CreateNewsPage extends StatefulWidget {
  const CreateNewsPage({Key? key}) : super(key: key);

  @override
  _CreateNewsPageState createState() => _CreateNewsPageState();
}

class _CreateNewsPageState extends State<CreateNewsPage> {
  TextEditingController newsContentController = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Create News'),
      ),
      body: Padding(
        padding: EdgeInsets.all(10),
        child: Form(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: <Widget>[
              Text('เนื้อหาใหม่'),
              TextFormField(
                controller: newsContentController,
              ),
              SizedBox(
                height: 20,
              ),
              SizedBox(
                width: double.infinity,
                child: ElevatedButton(
                  onPressed: () {
                    var newsContent = newsContentController.text;
                    print(newsContent);
                  },
                  child: Text('เพิ่ม'),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

```