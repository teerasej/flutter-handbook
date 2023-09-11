
# ปิดหน้า create news เวลากดปุ่ม

เปิดไฟล์ **lib/create_news_page.dart**

เราจะเรียกใช้ Navigator.pop() จากภายใน function ของปุ่ม ElevatedButton() ซึ่งจะเป็นการบอกให้แอพเปิดกลับไปหน้าที่แล้ว 

```dart
ElevatedButton(
    onPressed: () {
    print('do something');

    // สั่งให้ถอดหน้า Create News Page ออกจากแอพพลิเคชั่น
    Navigator.pop(context);
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
              TextFormField(),
              SizedBox(
                height: 20,
              ),
              SizedBox(
                width: double.infinity,
                child: ElevatedButton(
                  onPressed: () {
                    print('do something');
                    Navigator.pop(context);
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